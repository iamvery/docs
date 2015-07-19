---
name: Sending Email
---

Pakyow has built-in mechanisms for delivering views through email. Here's a basic example:

	ruby:
	Pakyow::Mailer.new(view_path).deliver_to("test@pakyow.org")
	# `deliver_to` also accepts an array of addresses

The view will be constructed just like it would be if it was being presented in a browser. You can also access the view for manipulation and binding:

	ruby:
	mailer = Pakyow::Mailer.new(view_path)
	mailer.view.scope(:foo).apply(some_data)
	mailer.deliver_to(["test@pakyow.org", "example@pakyow.org"])

Access to the [mail message](https://github.com/mikel/mail) object is also available:

	ruby:
	mailer = Pakyow::Mailer.new(view_path)
	mailer.message.subject = 'Pakyow Rocks!'
	mailer.message.add_file("/path/to/file.jpg")
	mailer.deliver_to("test@pakyow.org")

There are several configuration settings for Mailer. See [configuration](/docs/configuration) for more information.
