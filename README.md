# watsonx-challenge2023-product-track1
We mostly used the Jupyter notebook for the challenge.

> First task: customer satisfaction
sentiment analysis f1_micro_score=0.96

We tuned the model for sentiment analysis by creating instructions using a few-shot prompting approach with the training data provided. Using the prompt lab, we gradually added more examples to achieve a higher F1 micro score.
This is the prompt we used with a total of 10 examples:
-----
is the customer satisfied? Generate result in 1 or 0. 1 being satisfied and 0 is not satisfied.\n

comment: It was fine, I had no problem at all.
satisfaction:1

comment: We needed a minivan on very short notice to drive out of town to a funeral.  Enterprise staff worked hard to find us one, and they did. We smashed the car into a parking garage pole.  Since we had purchased the comprehensive insurance we didn't have to go through all of the B.S.
satisfaction:1

comment: Well since I used to work for a car rental company and at the time I rented I was still working with the company, I was treated with nothing but respect and received a free upgrade as well
satisfaction:1

comment: I do not understand why I have to pay additional fee if vehicle is returned without a full tank
satisfaction:0

comment: We needed a minivan on very short notice to drive out of town to a funeral.  Enterprise staff worked hard to find us one, and they did. We smashed the car into a parking garage pole.  Since we had purchased the comprehensive insurance we didn't have to go through all of the B.S.
satisfaction:1

comment: I would like the reps be knowledgeable about the immediate area around the rental agency and or have maps for the area available free of charge.
satisfaction:0

comment: Last time I rented a car was when I went skiing with my whole family. We got a Chevy Blazer. We didn't think it was as large as a Ford Explorer, so we asked to switch. The agent was very nice and gave us the Ford Explorer.
satisfaction:1

comment: My experience was positive. The thing I didnt like was returning the car with full tank. It was time consuming, but I didn't want to pay fill-up charge to the rental company.
satisfaction:1

comment: Person very friendly but only person working counter
satisfaction:1

comment: It was okay, we got the car quickly, which is the most important thing
satisfaction:1
-----

> Second task: classification
offer recommendation f1_micro_score=0.5555555555555556

This second step was more difficult as a simple prompt and a few examples were not performing well because of the use case and a lack of knowledge on the data. We couldn't find obvious links between comments and recommended offers. Even summarizing the customer comments from the training data for a given action didn't help.
We ended up building more elaborate instructions as follows, with one example for each type of recommended offer:
-----
Generate best offer to unsatisfied customer based on the problem they faced. Choose offer recommendation from the following list: On-demand pickup location, Free Upgrade, Voucher, Premium features.
Set the offer recommended to On-demand pickup location if the comment mentions car pick-up issue or location issue.
Set the offer recommended to Free upgrade if the comment mentions car issues.
Set the offer recommended to Voucher if the comment mentions below average customer service.
Set the offer recommended to Premium features if the comment mentions a need for an extra service.

comment: I do not  understand why I have to pay additional fee if vehicle is returned without a full tank.
offer recommended: Premium features

comment: [...]
-----
Using only the comment without the context of the customer situation limits the effectiveness of classification. We would incorporate the other fields into the model given the chance.
