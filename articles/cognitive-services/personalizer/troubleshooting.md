---
title: Troubleshooting - Personalizer
description: This article contains answers to frequently asked troubleshooting questions about Personalizer.
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: diberry
---
# Personalizer Troubleshooting

This article contains answers to frequently asked troubleshooting questions about Personalizer.

## Transaction errors

<details>
<summary><b>I get an HTTP 429 (Too many requests) response from the service. What can I do?</b></summary>

**Answer**:
If you picked a free price tier when you created the Personalizer instance, there is a quota limit on the number of Rank requests that are allowed. Review your API call rate for the Rank API (in the Metrics pane in the Azure portal for your Personalizer resource) and adjust the pricing tier (in the Pricing Tier pane) if your call volume is expected to increase beyond the threshold for chosen pricing tier.

</details>

<details>
<summary><b>I'm getting a 5xx error on Rank or Reward APIs. What should I do?</b></summary>

**Answer**:
These issues should be transparent. If they continue, contact support by selecting **New support request** in the **Support + troubleshooting** section, in the Azure portal for your Personalizer resource.

</details>

## Learning loop

<details>
<summary>
<b>The learning loop doesn't seem to learn. How do I fix this?</b></summary>

**Answer**: The learning loop needs a few thousand Reward calls before Rank calls prioritize effectively.

If you are unsure about how your learning loop is currently behaving, run an [offline evaluation](concepts-offline-evaluation.md), and apply the corrected learning policy.

</details>

<details>
<summary><b>I keep getting rank results with all the same probabilities for all items. How do I know Personalizer is learning?</b></summary>

**Answer**: Personalizer returns the same probabilities in a Rank API result when it has just started and has an _empty_ model, or when you reset the Personalizer Loop, and your model is still within your **Model update frequency** period.

When the new update period begins, the updated model is used, and you’ll see the probabilities change.

</details>

<details>
<summary><b>The learning loop was learning but seems to not learn anymore, and the quality of the Rank results isn't that good. What should I do?</b></summary>

**Answer**:
* Make sure you've completed and applied one evaluation in the Azure portal for that Personalizer resource (learning loop).
* Make sure all rewards are sent, via the Reward API, and processed.

</details>


<details>
<summary><b>How do I know that the learning loop is getting updated regularly and is used to score my data?</b></summary>

**Answer**: You can find the time when the model was last updated in the **Model and Learning Settings** page of the Azure portal. If you see an old timestamp, it is likely because you are not sending the Rank and Reward calls. If the service has no incoming data, it does not update the learning. If you see the learning loop is not updating frequently enough, you can edit the loop's **Model Update frequency**.

</details>

## Offline evaluations

<details>
<summary><b>An offline evaluation's feature importance returns a long list with hundreds or thousands of items. What happened?</b></summary>

**Answer**: This is typically due to timestamps, user IDs or some other fine grained features sent in.

</details>

<details>
<summary><b>I created an offline evaluation and it succeeded almost instantly. Why is that? I don’t see any results?</b></summary>

**Answer**: The offline evaluation uses the trained model data from the events in that time period. If you did not send any data in the time period between start and end time of the evaluation, it will complete without any results. Submit a new offline evaluation by selecting a time range with events you know were sent to Personalizer.

</details>


## Learning policy

<details>
<summary><b>How do I import a learning policy?</b></summary>

**Answer**:
Learn more about [learning policy concepts](concept-active-learning.md#understand-learning-policy-settings) and [how to apply](how-to-manage-model.md) a new learning policy. If you do not want to select a learning policy, you can use the [offline evaluation](how-to-offline-evaluation.md) to suggest a learning policy, based on your current events.

</details>

## Security

<details>
<summary><b>The API key for my loop has been compromised. What can I do?</b></summary>

**Answer**:
You can regenerate one key after swapping your clients to use the other key. Having two keys allows you to propagate the key in a lazy manner without having to have any downtime. We recommend doing this on a regular cycle as a security measure.

</details>

## Next steps

[Configure the model update frequency](how-to-settings.md#model-update-frequency)