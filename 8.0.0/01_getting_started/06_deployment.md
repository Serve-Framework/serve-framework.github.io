# Deployment

--------------------------------------------------------

* Server Setup
	* [Public Repository](#public-repository)
	* [Private Repository](#private-repository)
* [Webhooks](#webhooks)

--------------------------------------------------------

A Serve installation can be deployed to a live server depending on your setup and installation method. If you have followed the [Deployment Ready](/8.0.0/01_getting_started/01_installation#deployment-ready) installation instructions in this documentation, you will be able to deploy your Serve project and make live changes via Github.

If you are not running Serve on a live server, you can skip over this part of the documentation.

--------------------------------------------------------

### Public Repository

If you are using a public Githb repository, you can simply clone your Serve fork using the `HTTPS` method.

1. Complete the [Deployment Ready](/getting-started/installation#deployment-ready) installation method.

2. SSH into your server and navigate to the `public_html` or equivalent directory.

```bash
ssh webuser@example.com && cd /home/public_html
```

> When you login via SSH, you will need use your `domainuser` (the web user that runs `PHP` on your server). If you login as `root`, the default web user may not have the correct privileges to your git repository.

3. Clone your project's repository (your Serve fork). 

```bash
git clone https://github.com/<username>/<repository>.git .
```

4. Add a `.gitignore` file to your `app/configurations` directory to ensure any custom configuration settings you make are not overwritten.

```
*
!.gitignore
!defaults/*
```

--------------------------------------------------------

### Private Repository
If you are using a private Githb repository, you will need to clone your Serve fork using the `SSH` method. This can be little more complicated to setup but will ultimately make deployment a breeze in the long run.

1. Complete the [Deployment Ready](/getting-started/installation#deployment-ready) installation method.

2. SSH into your server and navigate to the `public_html` or equivalent directory.

```bash
ssh webuser@example.com && cd /home/public_html
```

> When you login via SSH, you will need use your `domainuser` (the web user that runs `PHP` on your server). If you login as `root`, the default web user may not have the correct privileges to your git repository.

3. Generate a new SSH key on your production server. Please follow the tutorial [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) on Github.com. You will need to store the SSH key paraphrase in your server's keychain or equivalent.

4. Add the SSH key to your Github account using the tutorial [here](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) on Github.com

5. Clone your project's repository (your Serve fork). 

```bash
git@github.com:/<username>/<repository>.git .
```

6. Add a `.gitignore` file to your `app/configurations` directory to ensure any custom configuration settings you make are not overwritten.

```
*
!.gitignore
!defaults/*
```

--------------------------------------------------------

### Webhooks

Now that you've setup your git repositories on both your local and remote environments, you'll want to start deploying changes to your server automatically.

Using [Github Webhooks](https://developer.github.com/webhooks/) you'll be able to automatically update any changes on your production server with your Github repository.

1. Follow the [instructions over on Github](https://developer.github.com/webhooks/creating/) to create a webhook for your repository. Choose an endpoint such as `https://example.com/github-webhook`. Take note of the provided secret token.

2. In your `app/configurations/application.php` file, paste the token under `deployment.token`.

3. In your Serve project, create a route to receive the webhook and deploy the changes.

```php
$Serve->Router->get('/github-webhook', function($request, $response, $next)
{
	$webhook = Serve::instance()->Deployment->webhook();

	if ($webhook->validate())
	{
		$webhook->deploy();
	}
});
```

4. Test your webhooks are being received by making a change on your local git repository and pushing it to Github.
