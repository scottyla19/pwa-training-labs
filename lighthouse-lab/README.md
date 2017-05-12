# Notes
1. Chrome uses manifest.json to know how to style and format some of the progressive parts of your app, such as the "add to homescreen" icon and splash screen.
2. Other browsers don't (currently) use the manifest.json file to do this, and instead rely on HTML tags for this information. While Lighthouse doesn't require these tags, we've added them because they are important for supporting as many browsers as possible.
