##What is iteration data?
According to [wikipedia](https://en.wikipedia.org/wiki/Iteration), _"Iteration is the act of repeating a process, either to generate an unbounded sequence of outcomes, or with the aim of approaching a desired goal, target or result. Each repetition of the process is also called an "iteration", and the results of one iteration are used as the starting point for the next iteration."_
In Rich Anchor Recommendation, we do a lot of iterative processes, each iteration has input and output data, output of an iteration will become input of another iteration process.

![File snapshot in github.com](https://git-scm.com/book/en/v2/book/01-introduction/images/snapshots.png)
Example about iteration data: File snapshot in github.com.

##Why do we use it?
Output of an iteration can be removed/modified while another process is reading it, so the iteration process use that output has wrong result or stop running.

For example, we do a crawler for a news site. We want multiple version of the output, so we need two main features:
 - Read the newest output
 - Return to some version of output
Simple solution is to create a soft link, called "current" which always point to newest version. Each time we want to come back to earlier version, we just reset the soft link to it.


##How to do it?
We will show a example about how to use iteration data in crawling eva.vn content. We have to get the set of newest url in eva.vn, include a set of information of a url: url link, tag, category, time post, event content of url.
Assume that you will save data in /home/thai/crawl/sites/eva.vn
The first, create two folder like yyyy_MM_dd_HH_mm_ss:
```
mkdir 2016_03_04_13_44_00
mkdir 2016_03_04_13_45_00
```
Create soft link current and next to these folder:
```
ln -s /home/thai/crawl/sites/eva.vn/2016_03_04_13_44_00 current
ln -s /home/thai/crawl/sites/eva.vn/2016_03_04_13_45_00 next
```

We implemented **CrawlerData** class extends from **IOIterationData** class.
**IOIterationData** class extends from **IterationData** class.
In **IOIterationData** class, we override two methods of **IterationData** class to create 4 folder (input, output, data, temp) in each version of iteration data. So, look like:
```
public IOIterationData(File root) {
		super(root);
		mkdirsNext();
	}

	@Override
	protected void mkdirsNext() {
		if (!next().isDirectory()) {
			try {
				super.mkdirsNext();				
				if (current() != null && current().exists()) {
					FileUtils.mkdirs(new File(next(), current().getName()));
				}
			} catch (IOException e) {
				throw new RuntimeException("Can't create next directory: " + e, e);
			}
		}
	}

	/**
	 * remove next data and create a new one
	 */
	public File refreshNext() throws IOException {
		super.refreshNext();
		mkdirsNext();
		return next();
	}
```

In **IterationData** class we have to create _current_ and _next_ soft link, they are _iterateCurrent()_ method and _iterateNext()_ method:
```
	protected void iterateCurrent() throws IOException {
		Files.deleteIfExists(current);
		Files.createSymbolicLink(current, next().toPath());
	}

	protected void iterateNext() throws IOException {
		Files.deleteIfExists(next);
		File newTarget = new File(root, getTimestampDateString());
		Files.createSymbolicLink(next, newTarget.toPath());
	}

	public synchronized void iterate() throws IOException {
		iterateCurrent();
		iterateNext();
	}

	public File refreshNext() throws IOException {
		if (next().exists()) {
			FileUtils.deleteRecursively(next());
		}
		iterateNext();
		return next();
	}
```
These structure follow the logic: new coming output will be written to "next" version, and current output will always be "current".

Final, after performing crawler for eva.vn, the result looks like this:
![Example folder](http://i.imgur.com/1UkiFQx.png)
To come back to a specific version, 2016_03_03_16_55_44 for example, just do this:
```
rm /home/thai/alezaa/current
ln -s /home/thai/alezaa/2016_03_03_16_55_44 /home/thai/alezaa/current
```

Now, in another process, just always access to the soft link "current"
```
readlink /home/thai/alezaa/current
ls  /home/thai/alezaa/current
```

##Conclude
Iteration data is a good solution to create, read and manage data, which have a lot of versions. But only with not too big data, because with a large of data, storing many versions can eat a lot of storage. Good solution is to limit number of the versions, we can create a process to delete obsolete versions.
Study case: Github also uses this method to [save snapshot](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics) of code commit, but with large files, they also have [other method](https://help.github.com/articles/versioning-large-files/) to save storage and bandwidth.