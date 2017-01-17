---
layout: project
type: project
image: images/alloy_logo.png
title: Alloy Collaboration Web App 
permalink: projects/alloy-collab
date: 2016
labels:
  - Meteor Web Framework
  - Javascript
  - MongoDB
  - Semantic UI
summary: A Meteor web app for allowing members of the UH community find and collaborate on projects together
---

<img class="ui image" src="../images/alloy_landing-page.png">
<div class="ui rounded images">
  <img class="ui image" src="../images/alloy_user-profile.png">
  <img class="ui image" src="../images/alloy_project-edit.png">
</div>

Alloy is a tool to help members of the UH community find projects and find people for projects of their own. Have an idea for a project that would look good on your resume? Want to do your own custom senior project? Or do you just want experiment and learn something new with other students who just want to do the same? Alloy helps you find a team that's right for what you want.

Alloy is built using the Meteor web framework (which allows for clean and obvious model-view-controller designs).

My roles in the project:

* Developed system for grouping related skills

One of my main roles for the project was creating a basic way to group skills entered by users in a way that was useful for making project recommendations to that user. I accomplished this using a weighted graph wherein each vertex was represented by an all-lowercase, whitespace-removed string (to enforce uniqueness of vertices for comparisons). When users added skills or created projects requesting skills, those skills would be added to the graph and have the weights of existing edges connecting them incremented. 

One of the main challenges of this part of the project was that  there were limitations on the types of objects that Meteor could store. If I were to use a traditional adjacency list of edge objects, whenever a vertex of the graph was accessed to poll for certain properties, every edge in that vertex's adjacency list would need to be converted back to edge objects from generic EJSON type objects. To avoid this inefficiency, I flattened the graph so that the vertices and edges were held in separate MongoDB collections and accessed through Javascript classes made for each collection (removing the need for an edge type). The challenge of this was deciding how these collections would need to interact with each other to present the same methods of a traditional undirected, weighted graph.           
	
* System for selecting and displaying project recommendations for users 

Once the weighted skill-graph was implemented, I was responsible for making recommendations to users of projects that would match their skill set or were related to their skill set. I did this by taking the adjacency lists of each skill-graph vertex in the user's entered skill set and converting it to a maximum priority queue (based on edge weights). The skills/vertices on the opposite end of the edge as the skill used to retrieve the adjacency list was taken to be a related skill. A limited number of randomized related skills were taken for each user skill and used to retrieve a limited number of randomized projects requesting those related skills (this was done as an attempt to fairly display the most relevant projects in a limited display area).

There were several notable inefficiencies with the way the recommendation system was implemented. One, the entire adjacency list for each skill in the user's skill set was loaded each time, only to get the few most heavily weighted edges. Secondly, some skills in the user's skill set had the same related skills, all of these skills were stored in a list and only later filtered for unique items before retrieving related projects. Finally, some skills used to retrieve related projects resulted in duplicate projects in the resulting list; this was handled by filtering for unique items, shuffling the list, and returning only the first few items. My thought on most of these inefficiencies, at the time, was that they would not present noticeable latency to the user or take up much space so long as the skill-graph remained relatively sparse and user's entered skill set remained small.     
	
* Initial implementation of dynamic content for ui elements
	
I was initially the main team member responsible for adding dynamic functionality to ui pages of the app.


[Project Home Page](https://alloyteams.github.io/)

