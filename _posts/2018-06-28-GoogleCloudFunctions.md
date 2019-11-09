---
layout: post
title: Google Cloud Functions
fullview: true
---

Just got back from the Google Faculty Institute in Sunnyvale, CA, and I had the pleasure of going with Mary Eberlein from ECE.

My biggest takeaway was learning about Google's new [<span style="color: blue">Cloud Functions</span>](https://cloud.google.com/functions/).

Amazon has something similar with [<span style="color: blue">AWS Lambda</span>a](https://aws.amazon.com/lambda/).

Cloud Functions let you write a JavaScript function that visiting a URL will trigger. You get **2,000,000** calls for free.

Here's a trivial example:

Go to [<span style="color: blue">Google Cloud</span>](https://cloud.google.com) and login with your Google credentials.

Sign up here [<span style="color: blue">Google Cloud Education</span>](https://cloud.google.com/edu/) for a Google Cloud Platform account.

Then,

Click on `CONSOLE` on the upper right.

Click on `Select a project` on the upper left.

Click on `NEW PROJECT` on the upper right.

Click on `CREATE`.

It takes a few minutes to get created, but after that you should be able to click on `Select a project` again and see your project. Click on it.

Click on `Create a Cloud Function`.

Click on `Create function`.

Click on `Create`.

Click on your new function (e.g. function-1).

Click on the `Source` tab.

Here is the sample code provided:

```
/**
 * Responds to any HTTP request that can provide a "message" field in the body.
 *
 * @param {!Object} req Cloud Function request context.
 * @param {!Object} res Cloud Function response context.
 */
exports.helloWorld = (req, res) => {
  // Example input: {"message": "Hello!"}
  if (req.body.message === undefined) {
    // This is an error case, as "message" is required.
    res.status(400).send('No message defined!');
  } else {
    // Everything is okay.
    console.log(req.body.message);
    res.status(200).send('Success: ' + req.body.message);
  }
};
```

Click on the `Testing` tab.

Click on `Test the function`.

It will take a few minutes, but eventually it will say:

```
No message defined!
```

In the `Triggering event` window, change:

```
{}
```

to

```
{"message": "hi"}
```

Click on `Test the function`, again.

Now, it will say:

```
Success: hi
```

Click on the `Trigger` tab.

Click on the `URL`.

For example:

[<span style="color: blue"><span style="color: blue">https://us-central1-poetic-abacus-208617.cloudfunctions.net/function-1</span></span>](https://us-central1-poetic-abacus-208617.cloudfunctions.net/function-1)

It will again show:

```
No message defined!
```

Add `?message:hi` to the **end** of the URL.

For example:

[<span style="color: blue"><span style="color: blue">https://us-central1-poetic-abacus-208617.cloudfunctions.net/function-1?message:hi</span></span>](https://us-central1-poetic-abacus-208617.cloudfunctions.net/function-1?message:hi)

And it will still show:

```
No message defined!
```

And that's because that kind of URL is an HTTP GET request, and we need a POST request.

Instead, let's change the source to notice an HTTP GET request.

Click on the `Source` tab again.

Change all occurrence of `body` to `query`:

```
/**
 * Responds to any HTTP request that can provide a "message" field in the query.
 *
 * @param {!Object} req Cloud Function request context.
 * @param {!Object} res Cloud Function response context.
 */
exports.helloWorld = (req, res) => {
  // Example input: {"message": "Hello!"}
  if (req.query.message === undefined) {
    // This is an error case, as "message" is required.
    res.status(400).send('No message defined!');
  } else {
    // Everything is okay.
    console.log(req.query.message);
    res.status(200).send('Success: ' + req.query.message);
  }
};
```

Try the URL again, and it will say:

```
Success: hi
```
