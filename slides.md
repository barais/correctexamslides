---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
download: true
highlighter: shiki
lineNumbers: false
info: |
  ## Official slides for CorrectExam

  Learn more at [corrigeExamFront](https://olivier.barais.fr/corrigeExamFront/)
drawings:
  persist: false
title: Correct Exam
---

# Correct Exam

Modern software architecture in practise

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://olivier.barais.fr/corrigeExamFront/" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# My mojo

https://www.linkedin.com/pulse/i-have-right-do-research-software-engineering-hafedh-mili/

> Parnas noted "I would never have realized the nature of the problem, unless I had been working on that project, reviewing development documents, and sitting at that lunch table". Well, minimally, I need to be able to understand the conversation at that lunch table!

So here is a practical definition of what "understanding the conversation" means in this context: **You have no credibility to do software engineering research unless you have  at least the development skills/vocabulary of your graduating bachelor students.**

## That is why: I enjoy building real software, doing consultancy, working with students, ...

---
layout: center
class: text-center
---

# The project: correct exam


---

# Highly inspired by GradeScope Solution

> Gradescope grading software allows students to receive faster and more detailed feedback on their work, and allows instructors to see detailed assignment and question analytics. It is an easy way to take submissions digitally in order to preserve the original work and allow for quick and easy viewing from anywhere.

<img src="https://files.helpdocs.io/u7xiir5ze4/articles/5uxa8ht1a2/1592940278603/grading-page-overview.png" class="absolute left-200px rounded shadow" style="width:60%;"/>

<!--<div class="absolute left-20px bottom-20px">
This is a left-bottom aligned footer
</div>-->

---

# Why building that piece of software ?

- Enable to correct exams during meetings 😀
- Save $5 per student copy
- Create an open source implementation of real software with complex architecture to have a case study for
  - experiments in software engineering research
   - explaining modern software architecture to students
- Try to keep the credibility (in my vision) to do research in software engineering

---

# The technical architecture

- [**Quarkus**](https://quarkus.io/) for the back (Java + native compilation through GraalVM)
- [**Angular**](https://angular.io/) for the front
  - [**pdf.js**](https://mozilla.github.io/pdf.js/) to play with pdf (exam, scan exam, feedback for students)
  - [**fabric.js**](http://fabricjs.com/) to draw on top of a pdf
  - [**opencv**](https://opencv.org/) in wasm within a web worker to analyse the scan
  - [**tensorflow JS**](https://www.tensorflow.org/js) with the browser for digit and letter recognition
  - ...
- [**Docker**](https://www.docker.com/) and [K8S](https://kubernetes.io) to deploy the back and the monitoring layer
- Front is hosted in a CDN to follow the [JamStack](https://jamstack.org/) architecture (currently github page)
- CI/CD using [**github action**](https://github.com/features/actions), [**dockerhub webhook**](https://docs.docker.com/docker-hub/webhooks/), and [**gowebhook**](https://github.com/adnanh/webhook)
- Pluggable architecture based on micro-services to integrate question type automatic correction (let developper use their own programming language to provides solutions that automatically correct some questions)

---

# Architecture overview

<img src="https://olivier.barais.fr/webinria/resources/framework/img2.png" class="absolute left-50px rounded shadow" style="width:90%;"/>

---

# Diagrams

<div class="grid grid-cols-2 gap-10 pt-4 -mb-6">
<div>

### Conceptual architecture

```plantuml {scale: 0.7}
@startuml


package "Front" {
  MSGBus - [Web Worker (OpenCV WASM)]
  [Angular Front] - MSGBus
}


package "Back" {
  [Back (Quarkus.io)]
}


database "MySql" {
  folder "CorrectExamDB1" {
    [CorrectExamDB]
  }
}


[Angular Front] --> [Back (Quarkus.io)]
[Back (Quarkus.io)] --> [CorrectExamDB]

@enduml
```
</div>


<div>

### deployed architecture

```plantuml {scale: 0.7}
@startuml

cloud "CDN (github page)" {
  [Front]
}

cloud {
  [Back (Quarkus.io)]
  [CorrectExamDB (Mysql)]
}

[Front] --> [Back (Quarkus.io)]
[Back (Quarkus.io)] --> [CorrectExamDB (Mysql)]

@enduml
```
</div>
</div>

---

## Business model

![](/model.png)


---

## JHipster to generate the application skeleton

> JHipster is a development platform to quickly generate, develop, & deploy modern web applications & microservice architectures.



- jhipster 7.7.0 for the front
- jhisper 6.10.15

> Manual alignement of the API evolution 😀 (@Djamel)

---

# Demo and code overview

- [demo](https://correctexam.github.io/)
- [repo back](https://github.com/correctexam/corrigeExamBack)
- [repo front](https://github.com/correctexam/corrigeExamFront)



---
layout: center
class: text-center
---

# Learn More

[Documentations](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)
