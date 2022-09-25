# CRON Support

--------------------------------------------------------

By default, Serve does not require any `CRON` jobs to function correctly. However, you may want to setup a cron runner on your server for various background tasks.

The Serve example App comes built with a built in route for an example CRON job.

Setup a CRON job on your server to a CRON route and use the `?key=YOUR_APPLICATION_SECRET` in the URL to validate it.
