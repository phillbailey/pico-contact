A simple contact form for the micro CMS Pico
=====================
You may find more info on [gidlov.com/code/pico_contact](http://gidlov.com/code/pico_contact)

This is a simple contact form for your Pico-project.

##Example
[Running example](http://gidlov.com/contact)

##Setup
We use the nice [Mandrill](http://mandrill.com) app in this plugin, which is free for up to 12,000 sends per month.
Mandrill is delivered via Composer so we have to update the `require` key of `composer.json` and add the following:

	"mandrill/mandrill": "1.0.*"

Run the `composer update` comand.

If you are receiving about Pico being redeclared, comment out (//) the following line in Pico's index.php:

        require(LIB_DIR .'pico.php');

Now open your `config.php` file and add insert this:

	$config['contact'] = array(
		'post' => $_POST,
		'send_to' => 'your_email',
                'mandrill_api_key' => 'your_mandrill_api_key'
	);

Create a contact.md page (or whatever you want to call it) and insert your contact form there, like this:

	<!--CONTACT-MESSAGE-->
	<form role="form" method="post" name="contact">
		<div class="form-group contact-name-class">
			<label for="name">Name*</label>
			<input type="text" name="name" class="form-control" id="name" placeholder="Enter Name">
		</div>
		<div class="form-group contact-mail-class">
			<label for="mail">Email address*</label>
			<input type="email" class="form-control" id="mail" name="mail" placeholder="Enter email">
		</div>
		<div class="form-group contact-subject-class">
			<label for="subject">Subject</label>
			<input type="text" class="form-control" id="subject" name="subject" placeholder="Subject">
		</div>
		<div class="form-group contact-message-class">
			<label for="message">Message*</label>
			<textarea class="form-control" id="message" name="message" rows="5"></textarea>
		</div>
		<button type="submit" class="btn btn-default" name="contact" value="true">Submit</button>
	</form>

That's it!

##Notice

`<!--CONTACT-MESSAGE-->` at the top of the form is used to display messages, so leave it alone (or move it). Also leave the `name` attribute of the form element as it is, and the `name` and `value` of the submit button.
The required fields are `namn`, `mail` and `message`.

##Tweaks

If your server is not configured to send mail, try to use SMTP instead by adding this to `config['contact']`:

	'smtp' => array(
		'host' => 'your_host',
		'username' => 'your_username',
		'password' => 'your_password',
		'port' => 587,
		'auth' => 'true',		// Enable SMTP authentication, true or false
		'encryption' => 'tls',	// Enable encryption, tls or ssl
	),

Define a custom subject like this:

	// %1$s = name, %2$s = mail, %3$s = input subject, %4$s = site_title, %5$s = base_url
	'subject' => 'New mail from %1$s sent via %4$s', 

Adding custom body data example:

	// %1$s = name, %2$s = mail, %3$s = input subject, %4$s = site_title, %5$s = base_url
	'body_header' => 'New mail from %4$s'."\n".'Name: %1$s'."\n".'Mail: %2$s'."\n".'Subject: %3$s'."\n".'Message:'."\n\n".'',
	'body_footer' => '',

Alert messages example:

	'alert_messages' => array(
		'error' => '<div class="uk-alert uk-alert-danger"><h2>Whops, error!</h2><p>Your message could not be sent. Sorry. %1$s</p></div>', //%1$s = PHPMailer error
		'validation_error' => '<div class="uk-alert uk-alert-danger"><h2>Try again!</h2><p>Your message could not be sent.</p><p>%1$s</p></div>', //%1$s = Validation errors
		'success' => '<div class="uk-alert uk-alert-success"><h2>Thanks for your message!</h2><p>I will reply as soon as possible.</p></div>',
	),

Validation messages example:

	'validation_messages' => array(
		'required' => '', 		// %1$s = field name
		'invalid_mail' => '',	// %1$s = field name
	),

A custom form input validation error class:

	'error_class' => 'form-danger',

##License

Contact is released under [LGPL](http://www.gnu.org/licenses/lgpl-3.0-standalone.html).