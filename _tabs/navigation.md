---
# the default layout is 'page'
layout: about
icon: fas fa-map
order: 4
---

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Table View</title>
  <style>
    table {
      width: 100%;
      border-collapse: collapse;
      text-align: left;
      font-family: Arial, sans-serif;
    }
    th, td {
      border: 1px solid #ddd;
      padding: 8px;
    }
    th {
      font-weight: bold;
    }
    tr:nth-child(even) {
    }
    tr:hover {
    }
    a {
      text-decoration: none;
      color:rgb(125, 125, 125);
    }
    a:hover {
      text-decoration: underline;
    }
  </style>
</head>
<body>
  <table>
    <thead>
      <tr>
        <th>Section</th>
        <th>Subsection</th>
        <th>Links</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td rowspan="7">Root Directories</td>
        <td>Home</td>
        <td><a href="/">Home</a></td>
      </tr>
      <tr>
        <td>Categories</td>
        <td><a href="/categories">Categories</a></td>
      </tr>
      <tr>
        <td>Tags</td>
        <td><a href="/tags">Tags</a></td>
      </tr>
      <tr>
        <td>Archives</td>
        <td><a href="/archives">Archives</a></td>
      </tr>
      <tr>
        <td>About</td>
        <td><a href="/about">About</a></td>
      </tr>
      <tr>
        <td>Navigation</td>
        <td><a href="/navigation">Navigation</a></td>
      </tr>
      <tr>
        <td>Login</td>
        <td><a href="/login">Login</a></td>
      </tr>
      <tr>
        <td rowspan="10">Projects</td>
        <td rowspan="2">Pi Molecular Research</td>
        <td><a href="/projects/crem">CReM</a></td>
      </tr>
      <tr>
        <td><a href="/projects/crem-basic-example">CReM Basic Example</a></td>
      </tr>
      <tr>
        <td>Spenditure</td>
        <td><a href="/projects/spenditure">Spenditure</a></td>
      </tr>
      <tr>
        <td rowspan="4">TripTailor</td>
        <td><a href="/projects/triptailor">TripTailor</a></td>
      </tr>
      <tr>
        <td><a href="/projects/triptailor-deepdives">TripTailor Deepdives</a></td>
      </tr>
      <tr>
        <td><a href="/projects/triptailor-contribution">TripTailor Contribution</a></td>
      </tr>
      <tr>
        <td><a href="/projects/triptailor-commits">TripTailor Commits</a></td>
      </tr>
      <tr>
        <td rowspan="2">Valiant Aerotech</td>
        <td><a href="/projects/valiant-aerotech">Valiant Aerotech</a></td>
      </tr>
      <tr>
        <td><a href="/projects/valiant-aerotech/mission-planner/overview">Mission Planner Overview</a></td>
      </tr>
    </tbody>
  </table>
</body>
</html>
