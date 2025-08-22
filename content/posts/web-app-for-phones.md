---
title: "Things to Keep in Mind When Building Web Apps for Phones"
date: 2025-08-22
---

## Introduction

When [building the Free Tabata app]({{< ref "free-tabata.md" >}}) recently, it was immediately clear to me that this app was designed to be primarily used on mobile devices.
While all such devices come with a pre-installed browser that can open the web app, the user experience isn't guaranteed to match that of laptops or desktop computers, which come with a physical keyboard and larger screens.

This post serves as a collection of things to keep in mind when developing such apps.

## Be mindful of whitespace

While generous whitespace looks great on larger screens, it can easily become tedious on mobile devices when users have to constantly scroll to reach parts of the application, or when content isn't even visible because it's placed outside of the screen boundaries.

## Don't rely on keys

Technically, it's possible to connect a Bluetooth keyboard to mobile devices and use hotkeys.
In practice, however, users rarely do this.
Instead, make sure all functionality is accessible through taps and think of hotkeys only as a feature for power users.

## Add an apple-touch-icon

One great trend in web apps recently is building them as [Progressive Web Apps (PWAs)](https://en.wikipedia.org/wiki/Progressive_web_app).
iOS devices, for example, provide an "Add to Home Screen" function that lets you add an icon to your home screen just like regular apps.
This doesn't use the site's favicon, however, but requires a dedicated `apple-touch-icon` which can be included in HTML like so:

```html
<head>
    <link rel="apple-touch-icon" href="favicon.png"/>
</head>
```

Tip: Sites like [favicon.io](https://favicon.io/) make creating different favicon types much more convenient.

## Consider a Wake Lock

Usually, phones and tablets are configured to turn off their screens to save energy much sooner than desktop devices.
This can prove especially annoying for use cases like [Tabata training](https://en.wikipedia.org/wiki/High-intensity_interval_training#Tabata_regimen), where the screen should stay on for the multiple minutes of the workout.

Fortunately, modern browsers provide a [Wake Lock](https://developer.mozilla.org/en-US/docs/Web/API/WakeLock) API that can prevent the device from turning off its screen as long as the lock is active.
