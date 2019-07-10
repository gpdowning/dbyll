---
layout: post
title: Canvas API
fullview: true
---

Every week I create several quizzes on Canvas with several questions each and with several multiple-choice answers for each question.

Using the Canvas Web GUI for this purpose has now become very, very tedious, with lots of pointing, clicking, copy, and pasting...

This should have occurred to me months ago, and that is that there is a [<span style="color: blue">Canvas RESTful API</span>](https://canvas.instructure.com/doc/api/) that I can use programmatically to create those quizzes, with their questions and their answers.

The first thing you need is a Canvas access token.

Login to Canvas, click on `Account` at the top of the left sidebar, and then click on `Settings`.

Scroll down to the `Approved Integrations` section, and click on `+ New Access Token`.

A window will pop up with a form. Fill in `Purpose` with a name for the access token, leave `Expires` blank, and click on `Generate Token`.

Another window will pop up with the token. Copy it immediately and store it in a safe place. Canvas will not show you that token again. If you lose it, you'll have to repeat this process to get another token.

The second thing you need is the Unix [<span style="color: blue">cURL</span>](https://curl.haxx.se) utility.

Now you can make a [<span style="color: blue">RESTful API request</span>](https://en.wikipedia.org/wiki/Representational_state_transfer) to Canvas.

Here's an example of a `curl` call to simply get a particular quiz from a particular course:

```
curl https://utexas.instructure.com/api/v1/courses/1202012/quizzes/1175681/ -H 'Authorization: Bearer <your access token>'
```

Where `1202012` happens to be my `course id` and `1175681` happens to be my `quiz id`.

You can get your `course id` and your `quiz id` by visiting a particular quiz of a particular course using the Canvas Web GUI.

The response to that `curl` call will be a [<span style="color: blue">JSON</span>](https://en.wikipedia.org/wiki/JSON) document with all of the attributes and corresponding values of that quiz.

Slightly more challenging is the `curl` call to create a quiz.

```
curl https://utexas.instructure.com/api/v1/courses/1202012/quizzes/ -d @Quiz.json -H 'Content-Type: application/json' -H 'Authorization: Bearer <your access token>'
```

Again, `1202012` is my `course id` and the file `Quiz.json` contains the JSON that configures the quiz the way that I want:

```
{
  "quiz": {
    "due_at": "2017-10-17T18:05:00Z",
    "lock_at": "2017-10-17T18:05:00Z",
    "quiz_type": "assignment",
    "time_limit": 4,
    "title": "Quiz #1",
    "unlock_at": "2017-10-17T18:00:00Z"
  }
}
```

Of course the Canvas API responds to many, many, more kinds of requests, and therefore is rich with opportunities to automate many other things.

Finally, if you're familiar with Python, there's a beautiful library for making API calls: [<span style="color: blue">requests</span>](http://docs.python-requests.org).
