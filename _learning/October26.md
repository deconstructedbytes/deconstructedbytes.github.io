---
title: "October 26, 2024"
collection: learning
type: "journaling"
permalink: /learning/October26
venue: ""
date: 2024-10-26
location: ""
---

Daily learning/experience notes for tracking and comprehension checks.


Reading and applying...
==
[Practical Threat Detection Engineering](https://github.com/PacktPublishing/Practical-Threat-Detection-Engineering)
==================

I'm a sucker for [Humble Bundle]() packages, I have accumulated an impressive library of technical resources that even the most ambitious learning will struggle to get through. Ignore my book hoarding, I recently grabbed the Threat Intelligence bundle from Humble and started reading "Practical Threat Detection Engineering" by Megan Roddie, Jason DeyalSingh, and Gary Katz.

In every role I've had in Cyber Security there has always been a component of creating a signature, alert, detection - whatever the popular term is at the time. Currently my team does this as a result of our Threat Hunting process one possible output from an effective hunt is new detection logic that can be used to detect threat actor behavior should it present in the environment in the future. While I do and have done this, other than reading and playing around with some Detection-as-Code material I haven't actually "learned" what it means to actual "engineer" detections. So my hope in picking up the book is to gain insight to be a better-versed and a more-cabable person who, sometimes creates detections.

The first few chapters of the book focus around introducing types of attacks (BEC, DOS, Malware), attack framing models (LHM Kill-chain, Att&ck, and Unified Kill-chain), and import definitions for detection engineering and relationships with other functions with a data-driven or IT organization. I'll admit to my recollection, this was the first time I've read about the Unified Kill Chain. The LHM Kill Chain, in my opinion is good for describing phases of an attack for a big picture perspective and small non-complex attacks. For more advanced attacks the attibuting phases to the kill chain can cause confusion i.e. if I detect malware excution on a system what phase of the kill chain did I capture?? I think the answer is different depending on the scope of the incident.

Important definitions from the book:

Detection Engineering -  A set of processes that enable potential threats to be
detected within an environment. These processes encompass the end-to-end life cycle, from
collecting detection requirements, aggregating system telemetry, and implementing and
maintaining detection logic to validating program effectiveness.

Staleness can be used to define the continued effectiveness or value of a detection
confidence of a detection measures the
probability that the alert is a true positive – that is, the alert is triggered under the expected conditions
noisiness of a detection identifies how often a
detection creates an alert that does not result in remediation



Looking ahead
======

If I'm being honest, I don't know if I'll read the entire book, I'd like to but time and prioritizies being what they are... The book deep dives into lab creation, capturing meaningful metrics, validation, career as a Detection Engineer guidance and more, which while I do find of interest aren't areas that I'm currently pursuing. I'm not consuming the information in hopes to become a Detection Engineer, but to better understand what DEs are doing, the advice they're following, how their pipelines work, the relationships between what they're doing and if there are any best practices I can adopt to the things my team is doing.

To that end I will read most of it. Lots of content around the pipelines, lifecycle and usage of threat intelligence. I'm curious about how the authors define best practices around using atomic IOCs vs Behavioral patterns for detections. I look forward to reading the goods.