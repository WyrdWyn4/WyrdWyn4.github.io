---
title: TripTailor
description: A platform designed to revolutionize travel planning by offering a seamless and personalized experience for users to create, share, and explore travel itineraries.
date: 2024-11-28 00:00:00 +0800
categories: [Projects, TripTailor]
tags: [triptailor, software design, microservices, golang, react.js, postgresql, gin, docker, jwt authorization]
pin: true
math: true
mermaid: true
image: assets/img/covers/trip-tailor/triptailor-overview-cover.png
author: 
permalink: /triptailor/overview
hidden: false
---

# TripTailor

## Overview

![logo](assets/img/logos/trip-tailor/triptailor-logo.png){: style="width: 300px; height: 300px; object-fit: cover; border-radius: 50%;" }

**TripTailor** is a platform designed to revolutionize travel planning by offering a seamless and personalized experience for users to create, share, and explore travel itineraries. Combining personalization, community engagement, and intuitive features, TripTailor empowers users to craft itineraries tailored to their unique interests, demographics, and budgets.

## Features

- **Personalized Itineraries**: Create travel plans with detailed information such as destinations, events, timings, and budgets.
- **Community Engagement**: Like, comment, and save itineraries into user-defined boards.
- **Advanced Search**: Dynamic search functionality with tag-based filtering and ranking for relevant results.
- **Scalable Architecture**: Microservice-based design for modularity, fault tolerance, and scalability.
- **Profile Customization**: Tailor profiles with travel interests, languages spoken, and tags for tailored recommendations.

## Technology Stack

### Frontend
- **React.js**: Component-driven framework for building dynamic user interfaces.
- **JavaScript (ES6+)**: Used for frontend logic and interaction handling.
- **CSS**: Modular and responsive styling for consistent design.

### Backend
- **Golang**: Efficient, high-performance language for backend logic.
- **Gin Framework**: RESTful API creation with lightweight and fast routing.
- **Docker**: Containerized services for consistent environments and easy deployment.

### Database
- **PostgreSQL**: Relational database supporting JSONB for structured and semi-structured data.
- **PgAdmin**: GUI for managing and monitoring the database.

## Services Architecture

### Microservices
- **Main Service**: Central service for database management and backend maintenance.
- **Authentication Service**: Secure user signup, login, and JWT-based authorization.
- **Profile Service**: Handles user profile creation and updates.
- **Itinerary Service**: Manages creation, storage, and retrieval of travel itineraries.
- **Feed Service**: Displays curated itineraries based on user-selected tags.
- **Search Service**: Enables advanced itinerary search with relevance-based ranking.
- **Save Service**: Allows users to save and manage boards with curated itineraries and posts.

---

TripTailor combines innovation and simplicity to create an enjoyable and efficient travel planning experience for users, making their journey from planning to execution as seamless as possible.

## Team

<div style="display: flex; justify-content: space-around; align-items: center;">
    <div style="text-align: center;">
        <img src="assets/img/people/Waleed Mannan Khan Sherwani.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/WyrdWyn4" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Waleed Khan Sherwani</p>
        </a>
    </div>
    <div style="text-align: center;">
        <img src="assets/img/people/Ally Mackenzie Reid.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/allymreid" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Ally Mackenzie Reid</p>
        </a>
    </div>
    <div style="text-align: center;">
        <img src="assets/img/people/Miguel Diego Pond.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/MiguelPond" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Miguel Diego Pond</p>
        </a>
    </div>
</div>

<div style="display: flex; justify-content: space-around; align-items: center;">
    <div style="text-align: center;">
        <img src="assets/img/people/Léo Gilbert.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/Leo-Gilbert" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Léo Gilbert</p>
        </a>
    </div>
    <div style="text-align: center;">
        <img src="assets/img/people/Jordyn Elizabeth O'Brien.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/jordyob03" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Jordyn Elizabeth O'Brien</p>
        </a>
    </div>
    <div style="text-align: center;">
        <img src="assets/img/people/Abdulaziz Turonov.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/AbdulTur" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Abdulaziz Turonov</p>
        </a>
    </div>
</div>

<div style="display: flex; justify-content: space-around; align-items: center;">
    <div style="text-align: center;">
        <img src="assets/img/people/Noah James Colbourne.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/noahjrc" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Noah James Colbourne</p>
        </a>
    </div>
    <div style="text-align: center;">
        <img src="assets/img/people/Naomi Ann Pierce.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/napierce" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Naomi Ann Pierce</p>
        </a>
    </div>
    <div style="text-align: center;">
        <img src="assets/img/people/Mitch C. Roberts.png" alt="Team Member" style="width: 150px; object-fit: cover; border-radius: 50%;" class = "team-member-img">
        <a href="https://github.com/MitchRoberts" target="_blank">
            <p style="text-align: center; font-weight: smaller; margin-top: 0;">Mitch C. Roberts</p>
        </a>
    </div>
</div>

> Checkout the [TripTailor GitHub Repository](https://github.com/WyrdWyn4/TripTailor), and the [Deepdive Article](/triptailor/detail) for a detailed look into the project.
{: .prompt-tip}

> In addition, a great [presentation](../assets/docs/projects/triptailor/ptojects/triptailor/TripTailor Presentation.pptx) from the team is available for download!
{: .prompt-tip}

> **Note**: This project was developed as part of the [ECE 6400](https://www.mun.ca/university-calendar/st-johns-campus/faculty-of-engineering-and-applied-science/11/3/#d.en.365116) Software Development Course at the Memorial University of Newfoundland.
{: .prompt-info}

<style>
  .team-member-img {
    max-height: 150px;
    max-width: 150px;
    border-radius: 50%;
  }

  .logo-img {
    max-height: 300px;
    max-width: 300px;
  }

  .team-member-img + a p {
    font-size: 16px;
  }

  @media (max-width: 600px) {
    .team-member-img + a p {
      font-size: 12px;
    }
  }

  @media (max-width: 400px) {
    .team-member-img + a p {
      font-size: 10px;
    }
  }
</style>

<script>
  function adjustImages(className) {
    setTimeout(() => {
      const images = document.querySelectorAll(`.${className}`);
      let globalMin = Infinity;

      images.forEach(img => {
        const rect = img.getBoundingClientRect();
        const minDim = Math.min(rect.width, rect.height);
        globalMin = Math.min(globalMin, minDim);
      });

      console.log('Global min:', globalMin, 'for class:', className);

      images.forEach(img => {
        img.style.width = `${globalMin}px`;
        img.style.height = `${globalMin}px`;
        img.style.borderRadius = '50%';
      });
    }, 1000);

    console.log('Images adjusted for class:', className);
  }

  adjustImages('team-member-img');
  adjustImages('logo-img');
</script>
