---
title: Acknowleding actions
slug: action-acknowledge
order: 6
---

<div class="section-content">
Actions must **always** be acknowledged using the `ack()` function. This lets Slack know that the action was received and updates the Slack user interface accordingly. Depending on the type of action, your acknowledgement may be different. For example, when responding to a dialog submission you will call `ack()` with validation errors if the submission contains errors, or with no parameters if the submission is valid.

We recommend calling `ack()` right away before sending a new message or fetching information from your database since you only have 3 seconds to respond.
</div>

```javascript
// Regex to determine if this is a valid email
let isEmail = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/
// This uses a constraint object to listen for dialog submissions with a callback_id of ticket_submit 
app.action({ callback_id: 'ticket_submit' }, ({ action, ack }) => {
	// it’s a valid email, accept the submission
  if (isEmail.test(action.submission.email)) {
	  ack();
  } else {
		// if it isn’t a valid email, acknowledge with an error
	  ack({
		  errors: [{
			  "name": "email_address",
			  "error": "Sorry, this isn’t a valid email"
      }]
    });
  }
});
```