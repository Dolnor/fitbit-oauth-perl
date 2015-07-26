# What is this?

This is a fork of a script by Christopher Bowns, @cbowns to upload WeightBot weights to Fitbit. This fork re-purposes it to upload data from bluetooth-connected MiScale instead. 

It's actually two Perl scripts: one to request a read-write OAuth token from Fitbit, and another to upload a CSV file extracted from Mi Fit Android app. 

## What You'll Need:

1. [OAuthSimple.pm](https://raw.github.com/jrconlin/oauthsimple/master/perl/OAuthSimple.pm). Download it and put it right next to `request_token.pl` and `upload_data.pl`.

2. Your [MiScale CSV data](https://github.com/Dolnor/mifit-data-export). Download and extract to the same folder as this tool, follow readme on how to set up and use it.

3. A [registered Fitbit application](https://dev.fitbit.com/apps) with read-write access, and its API secrets.

Original README follows ...

## How To Do It:

### 1. Get set up with Fitbit and its API keys.

#### Register an application with Fitbit:

Go to https://dev.fitbit.com/apps/new and register a new application. Set "Application Type" to "Desktop". Leave "Callback URL" blank. Set "Default Access Type" to "Read & Write".

#### Save the API keys:

Make a file in your home directory named `.api_keys`:

	touch ~/.api_keys

View your application on [dev.fitbit.com](https://dev.fitbit.com/apps). The details page will have entries for "Consumer key" and "Consumer secret". Copy those into your `.api_keys`, one per line:

	> cat .api_keys
	…
	fitbit_uploader_oauth_consumer_key=<consumer key>
	fitbit_uploader_oauth_shared_secret=<consumer secret>
	…

### 2. Request an OAuth token:

With an application and a Fitbit account, `request_token.pl` can request OAuth access on your behalf and spit out a read-write token and secret.

(If you haven't already, download [OAuthSimple.pl](https://raw.github.com/jrconlin/oauthsimple/master/perl/OAuthSimple.pm) so that Perl can talk to Fitbit.)

Run `request_token.pl`: `perl request_token.pl`. It will open a webpage that requests OAuth read-write access for your Fitbit application. Select "Allow". Fitbit will display a long PIN. Copy and paste that PIN into the terminal where `request_token.pl` is waiting, and hit enter.

`request_token.pl` will finish handshaking with Fitbit, and will print out:

	OAuth Token:
	<oauth token>
	OAuth Token Secret:
	<oauth token secret>

Add these into `.api_keys`. When you're done, it should look like:

	> cat .api_keys
	…
	fitbit_uploader_oauth_consumer_key=<consumer key>
	fitbit_uploader_oauth_shared_secret=<consumer secret>
	fitbit_uploader_oauth_token=<oauth token>
	fitbit_uploader_oauth_token_secret=<oauth token secret>
	…

For example,

	…
	fitbit_uploader_oauth_consumer_key=abcd1234
	fitbit_uploader_oauth_shared_secret=ef0123ab
	…

### 3. Run `upload_data.pl`.

`perl upload_data.pl` will toss everything at Fitbit's API.

What you have in your Weightbot CSV file is up to you. There's an early return clause in `upload_data.pl` if you only need it to run for a few days. It's safe to run the script multiple times: in my testing, Fitbit won't duplicate the posts, and the last-uploaded entry for that date wins.
