---
layout: post
title: "Setting up user accounts on Meteor with Foundation"
comments: True
---

This is a tutorial showing you how to set up user accounts on Meteor with the User Accounts package, namely the Foundation one (`useraccounts:foundation`). I've spent an inordinate amount of time on this particular issue. The fact of the matter is, I'm relatively new to this whole Meteor thing and Foundation being much less popular than Bootstrap doesn't help at all.

Yes, I could write the sign-in/sign-up templates myself, but that would not make a lot of sense when a group of people already came up with a way for me to do it in a much easier way.

In short, what you need to do is to clone the package into the `/packages` folder in your meteor project path. Now you can work with customizing it. There are several states available for this app:

`changePwd`

    "changePwd", // Change Password
    "enrollAccount", // Account Enrollment
    "forgotPwd", // Forgot Password
    "hide", // Nothing displayed
    "resetPwd", // Reset Password
    "signIn", // Sign In
    "signUp", // Sign Up
    "verifyEmail", // Email verification

AT.prototype.ROUTE_DEFAULT = {
    changePwd:      { name: "atChangePwd",      path: "/change-password"},
    enrollAccount:  { name: "atEnrollAccount",  path: "/enroll-account"},
    ensureSignedIn: { name: "atEnsureSignedIn", path: null},
    forgotPwd:      { name: "atForgotPwd",      path: "/forgot-password"},
    resetPwd:       { name: "atResetPwd",       path: "/reset-password"},
    signIn:         { name: "atSignIn",         path: "/sign-in"},
    signUp:         { name: "atSignUp",         path: "/sign-up"},
    verifyEmail:    { name: "atVerifyEmail",    path: "/verify-email"},
};