# XSS with sandbox escape

My friend Andrew shared an interesting SANS article with me:
Source: [Azure 0day Cross-Site Scripting with Sandbox Escape](https://pen-testing.sans.org/blog/2016/08/19/azure-0day-cross-site-scripting-with-sandbox-escape)

## Notes

### Introduction

* Keep an eye out for Chris Dale’s work
* The attacker needed to compromise an Azure web-app
* The vulnerability allows an attacker to attack any administrator of the SaaS platform

### Using Microsoft Azure as your Cloud Platform

* Azure is Microsoft’s public cloud platform and they provide:
  1. Infrastructure as a service
  2. Platform as a service
  3. Software as a service and more…
* Because of the Azure service architecture, other clients can be compromised if one
customer has a security breach
* Kudu provides debugging tools and lets you spawn command shells and PowerShell to help troublehsoot

### Breaching the Sandbox

* The attack takes place on the Kudu administration panel
* Attempted to craft a filename with Javascript code in the name: think
`<script>alert('xss');</script>.exe`
* Filenames can’t have special characters like `<` or `>` so he looked elsewhere
* Opening up the properties of a process will show the entire environment of the process, including environment variables
* Setting an environment variable through a specially crafted request URL allows for persistent XSS

[Back](./index.html)
