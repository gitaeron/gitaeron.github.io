---
title: "pyttsx3 setProperty IndexError"
date: 2023-04-18T21:55:00+08:00
author: "Aeron"

readingtime: 1
categories: ["Technology"]
tags: ["pyttsx3", "translate by chatgpt-3.5"]
---

# IndexError with setProperty('voice', value)

{{<toc>}}

<!--more-->

---

## Why does this error occur 

Essentially, the "id" attribute of the pyttsx instance object is the sequence of voice packages in the registry, and when you use the setProperty() method like this:

pyttsx3_obj.setProperty('voice', value)


You use name=voice to specify a certain voice package, and the value parameter passes in the sequence ID generated after obtaining the system voice package in pyttsx3.voice property. It is called by pyttsx3_obj.id, so the erroneous behavior here is IndexError, which means that the index is out of bounds.

In short, the voice package in the registry is missing, so you have an index out of bounds error.

---

## How to Solve

Actually, it's quite simple. The voice package registry path that pyttsx3 reads is:

- HKEY_LOCAL_MACHINESOFTWAREMicrosoftSpeechVoicesTokens

When you search for this error, some articles may mention this path: HKEY_LOCAL_MACHINESOFTWAREMicrosoftSpeech_OneCoreVoicesTokens, but you don't have to worry about it.

Press the win + R shortcut key, type regedit to open the registry editor, then open the path mentioned above, and you will see the number of downloaded voice packages. The index starts at 0, so here's how you solve the problem: ^_^*

If you don't need a specific voice package, just

- select a voice

- right-click and choose export

- modify the content of the exported regedit registry file (just change the name of the voice package inside)

- run the modified regedit registry file

Then, the index will not be out of bounds anymore.

---

## Another scenario

However, when you check the registry and find that the index is not out of bounds (the number of voice packages is correct), then you are facing another problem. Please open the link below.

Supported synthesizers (https://pyttsx3.readthedocs.io/en/latest/support.html)

---

# Refer

- pyttsx3 official document (https://pyttsx3.readthedocs.io/en/latest/index.html)

- pyttsx3 is not changing the voice? What to do now? (https://github.com/nateshmbhat/pyttsx3/issues/187)

- Supported synthesizers (https://pyttsx3.readthedocs.io/en/latest/support.html)

The number of tokens used this time: 1353
