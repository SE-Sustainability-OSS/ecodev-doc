# Send emails

Ecodev-core provides a generic method to send emails.

In order for it to work, one has to specify the following environment variables:

- `email_smtp`: the smtp address of your smtp provider (`smtp.gmail.com `) for gmail for instance
- `email_sender`: the email address to be used to send emails.
- `email_password`: the `email_sender` password.  <a href="https://www.gmass.co/blog/gmail-smtp/" class="external-link"  target="_blank">Documentation</a>
on how to do it via gmail.

Once this is done, the method signature looks like so:

```python
send_email(email: str, body: str, topic: str, images: Dict[str, Path]) -> None:
```

where:

- `email` is the email address to which to send the email
- `body` is the mail body to send.  This can be a html file rendered with Jinja2
- `topic`: is the email title
- `images`:  if any, the Dict of image tags:image paths to incorporate in the email