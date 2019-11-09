---
layout: post
title: Teaching Tools
fullview: true
---

I wanted to share with you some teaching tool ideas.

### Canvas Apps -> Instapoll

There's a new item on the Canvas left sidebar, called UT Canvas Apps.

Within it, you might want to try UT Instapoll. It's free and does most of what iClicker does.

### Canvas' New Gradebook

In the Canvas Gradebook, assignments and quizzes that were not submitted don't enter into the calculation of the total grade. To fix that you can go to the grade book settings and click on Treat Ungraded as 0.

I prefer to go to an assignment or quiz after its due date and in the setting of that assignment or quiz, click on Set Default Grade and set that to 0. That has the added benefit of e-mailing the student that I have just updated their grade to a 0.

Under Settings -> Feature Options, you can now turn on Canvas' New Gradebook. Within the settings for that, there is now the option of Late Policies. Within that, you can automatically apply a grade for missing submissions. That now frees you from having to do it for every assignment or quiz.

### Docker

My programming projects require several tools besides a compiler or interpreter, for example, a unit-test tool, a coverage tool, a static analyzer, etc.

I now create a Docker image for each of my classes and push that image to DockerHub.

Students can then pull that image from DockerHub, and no longer have to install that toolchain on their local boxes.

HackerRank

HackerRank allows you to post programming problems in many languages with corresponding tests to verify correctness. I'm now using HackerRank for most of my programming assignments.

### Zoom

Zoom is a video-conferencing tool. It's free for meetings of up to 100 people for up to 40 min. It's easy to use, has good quality sound and video, you can share your screen, and you can use a virtual whiteboard.

I'm using it to hold virtual office hours as a supplement to my in-person office hours.

In both of my classes, *CS371p: Object-Oriented Programming* and *CS373: Software Engineering* I've had my students use *source control*, *containerization*, and *continuous integration* for some time.

For source control I've been using [git](https://git-scm.com) and [GitHub](https://github.com), for containerization I've been using [Docker](https://www.docker.com), and for continuous integration I've been using [Travis CI](https://travis-ci.org)

In June of this year, [Microsoft](https://www.microsoft.com) acquired GitHub, which led to the exodus of many developers.

I wanted to see what else was out there and came upon [GitLab](https://gitlab.com).

GitLab is very similar to GitHub in many ways, but it has one outstanding feature, which is that continuous integration is built-in.

Furthermore its continuous integration offering makes use of Docker, which is an additional advantage.

With GitHub, Docker, and Travis CI, I needed to configure Docker and Travis CI in very similar and therefore redundant ways.

An example `.travis.yml` file for Python was:

```
language: python

python:
   - "3.6"

install:
    - pip install autopep8
    - pip install coverage
    - pip install mypy
    - pip install numpy
    - pip install pylint

script:
    make
```

And an example `Dockerfile` file for Python was:

```
FROM python:3

RUN pip install autopep8      && \
    pip install coverage      && \
    pip install mypy          && \
    pip install numpy         && \
    pip install pylint

CMD bash
```

Furthermore, `.travis.yml` doesn't accommodate multiple languages.

GitLab CI, on the other hand, is configured precisely by relying on Docker!

So, now:

`Dockerfile` for Python:

```
FROM python:3

RUN pip install autopep8      && \
    pip install coverage      && \
    pip install mypy          && \
    pip install numpy         && \
    pip install pylint

CMD bash
```

`Dockerfile` for JavaScript:

```
FROM node

RUN npm install    assert   && \
    npm install    lodash   && \
    npm install -g istanbul && \
    npm install -g jshint   && \
    npm install -g mocha

CMD bash
```

And now, `gitlab-ci.yml` makes use of both:

```
node:
    image: gpdowning/node
    script:
    - make

python:
    image: gpdowning/python
    script:
    - make
```
