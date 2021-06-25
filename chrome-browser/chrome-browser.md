# Chrome browser

## Print emulate: How to style and debug a Print web page

- [Ref: Stackoverflow](https://stackoverflow.com/questions/726825/how-do-you-debug-printable-css/36267984#36267984)

1. Open devtools and click on the "..." menu
2. click More Tools > Rendering settings to open the Rendering tab of the developer console (alternatively, if the console is open just navigate to this tab).
3. In that tab, select and check the box for Emulate Media: print.

![Emulate print](https://i.stack.imgur.com/mV2ra.png)

## How to gain access to HTTP domains without force redirecting to HTTPS

1. Go to chrome://net-internals/#hsts
2. Enter your url, example `localhost` under Delete domain security policies and press the Delete button.
3. Go to your localhost:4200 or whatever, and refresh.
4. Obs: You might need to go to chrome://settings/clearBrowserData, tick the box Cached images and files and press click the button Clear data.
