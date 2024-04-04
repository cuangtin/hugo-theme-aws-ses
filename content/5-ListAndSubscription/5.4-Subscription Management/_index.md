---
title : "Subscription Management"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---

{{% notice note %}}
This lab can only be done using the Amazon SES API and not in the Amazon SES console.
{{% /notice %}}

### Scenario
AWSomeNewsletter is a popular newsletter that keeps its subscribers informed about various topics such as TechNews, HealthWellness, TravelTips, and PersonalFinance. The subscribers have different interests, and some of them may not want to receive updates on all topics. To provide a personalized experience to its audience and comply with email marketing best practices, AWSomeNewsletter decides to implement subscription management using Amazon Simple Email Service (SES).

In this lab, you will learn how to use Amazon SES's subscription management capabilities to manage your email list subscriptions. You will learn how to set up ListManagementOptions in the SendEmail operation request to automatically enable unsubscribe links in your emails, and how to use the `{{amazonSESUnsubscribeUrl}}` placeholder to add an unsubscribe footer link to your emails.

### Set up ListManagementOptions
To enable subscription management for your emails, you need to include the ListManagementOptions header in your SendEmail operation request. This header specifies the contact list name and topic name, which Amazon SES uses to enable unsubscribe links in your emails.

Run the following AWS CLI command to send the email:
```
aws sesv2 send-email \
    --from-email-address sender@example.com \
    --destination '{"ToAddresses":["recipient@example.com"]}' \
    --content '{"Simple":{"Subject":{"Data":"Test email"},"Body":{"Text":{"Data":"This is a test email"},"Html":{"Data":"<html><body><p>This is a test email</p></body></html>"}}}}' \
    --list-management-options '{"ContactListName":"MyContactList", "TopicName":"MyTopic"}'
```
In the `ListManagementOptions` section, replace `MyContactList` with the name of the contact list (e.g. AWSomeNewsletterContactList), and MyTopic with the name of a topic that you created in you created in [Lab 5.3](../5.3-list-management).

You should receive a test email, which is just a plain HTML for now.

### Add an Unsubscribe Footer Link
To allow your recipients to unsubscribe from your emails, you need to include an unsubscribe footer link in your emails. You can use the `{{amazonSESUnsubscribeUrl}}` placeholder to add the unsubscribe URL to your emails.

Modify AWS CLI --content attribute to include the handlebar placeholder:
```
aws sesv2 send-email \
    --from-email-address sender@example.com \
    --destination '{"ToAddresses":["recipient@example.com"]}' \
    --content '{"Simple":{"Subject":{"Data":"Test email"},"Body":{"Text":{"Data":"This is a test email.\n\nTo unsubscribe, click {{amazonSESUnsubscribeUrl}}."},"Html":{"Data":"<html><body><p>This is a test email.</p><p>To unsubscribe, click <a href=\"{{amazonSESUnsubscribeUrl}}\">here</a>.</p></body></html>"}}}}' \
    --list-management-options '{"ContactListName":"MyContactList", "TopicName":"MyTopic"}'
```
Similarly, in the `ListManagementOptions` section, replace `MyContactList` with the name of the contact list (e.g. AWSomeNewsletterContactList), and MyTopic with the name of a topic that you created in you created in [Lab 5.3](../5.3-list-management).

Now, check your email and click on the unsubscribe link generated by Amazon SES. You should be directed to a Landing Page that looks similar to below.
<!-- image -->
{{% notice note %}}
For some mail applications, you'd need to mark the email as safe first before you can click on embdedded links.
{{% /notice %}}

### Conclusion
Subscription management is a powerful tool that allows you to manage your email list subscriptions automatically. By using the ListManagementOptions header and the `{{amazonSESUnsubscribeUrl}}` placeholder, you can enable unsubscribe links in your emails and give your recipients the ability to manage their email preferences.