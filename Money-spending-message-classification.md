

## Problem

We have an interesting talk with [Money Lover](https://moneylover.me/). It's an money/finance management for people. Using this app, an user will note a message each time they spend money. To serve their users better, Money Lover wants to know what kind of service/product category of users' spent money.

![](https://blogdotrichanchordotcom.files.wordpress.com/2016/05/finance_classify.jpg)

Fortunately, we have a nice classification method for short message. Then Money Lover and us decided to try it.

## Solution

### Prepare catalog tree

The catalog tree Money Lover shared us is great. It's perfect for financial management purpose, we think.

In this post, we introduce classification method and demo for the parent categories and Vietnamese messages.

```
{
    "categories": {
        "auto_and_transport": [
            "car_rental",
            "gas_and_fuel",
            ...
        ],
        "bills_and_utilities": [
            "internet",
            "phone",
            ...
        ],
        "business_services": [
            "advertising",
            "office_supplies",
            ...
        ],
        "education": [
            "books_and_supplies",
            ...
        ],
        "entertainment": [
            "games",
            "movies_and_music",
            ...
        ],
        "fees_and_charges": [
            "provider_fee",
            "loans",
            ...
        ],
        "food_and_dining": [
            "alcohol_and_bars",
            "cafes_and_restaurants",
            "groceries"
        ],
        "gifts_and_donations": [
            "charity",
            "gifts"
        ],
        "health_and_fitness": [
            "doctor",
            "personal_care",
            ...
        ],
        "home": [
            "home_improvement",
            "home_services",
            ...
        ],
        "income": [
            "bonus",
            "investment_income",
            ...
        ],
        "insurance": [
            "car_insurance",
            "health_insurance",
            ...
        ],
        "kids": [
            "allowance",
            "babysitter_and_daycare",
            ...
        ],
        "pets": [
            "pet_food_and_supplies",
            "pet_grooming",
            ...
        ],
        "shopping": [
            "clothing",
            "electronics_and_software",
            ...
        ],
        "transfer": [],
        "travel": [
            "hotel",
            "transportation",
            ...
        ],
        "mobile_app": [],
        "withdraw": [],
        "interest": [],
        "uncategorized": []
    }
}
```

### Mining linguistic data for categories

By doing some data mining technique, we can extract text/keywords related to each categories, for example:
```
"auto_and_transport": uber, grab, taxi, vnairlines, xăng, gửi xe, ...
"food_and_dining": metro, bigc, kfc, pizza, thịt, cá, trứng, ...
"entertainment": cgv, platinum, xem phim, game, quẩy, ...
...
```

One of our technique to extract text/keywords for a category was introduced at [this blog](https://blog.richanchor.com/2016/01/15/topic-modeling-with-lda/)

### Using mined data for Naive Bayes classification

We love [Naive Bayes](https://en.wikipedia.org/wiki/Naive_Bayes_classifier) technique, it's simple and extremely effective for short messages - where the keywords decide meaning of of message. Further detail about this method we had described in [this post](https://blog.richanchor.com/2015/12/23/predict-category-of-a-given-text-how-to-build-a-simple-naive-bayes-classifier-to-do-that/).

## Here come the result

We have a demo working [here](https://richanchor.com/messageClassify/?f=demo&message=an%20trua), just input the text in and see the result. Remember about the context of messages: they are financial short note.

Message: "[an trua](https://richanchor.com/messageClassify/?f=demo&message=an%20trua)" - (ăn trưa - lunch)
```
{
  "labels": [
    "food_and_dining"
  ]
}
```

## Future works

Current input of this classification is linguistic data from social channel (because financial message's context is similar to normal text context). One important data channel is not used: real users feedback on classified results.

In the future, we need to listen feedback of users who saw the classification result and rechecked it, then add that feedback info as learning data to make the classifier getting better day by day.