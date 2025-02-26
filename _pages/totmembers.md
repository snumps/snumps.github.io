---
layout: page
permalink: /totmembers/
title: 역대 회원
---
<div id="totmembers">
  {% assign all_semesters = "" %}
  
  {% for member in site.members %}
    {% for semester in member.semester %}
      {% unless all_semesters contains semester %}
        {% assign all_semesters = all_semesters | append: semester | append: "," %}
      {% endunless %}
    {% endfor %}
  {% endfor %}
  
  {% assign all_semesters = all_semesters | split: "," %}
  {% assign all_semesters = all_semesters | uniq %}

  <!-- Sort semesters in reverse alphabetical order -->
  {% assign all_semesters = all_semesters | sort %}
  {% assign all_semesters = all_semesters | reverse %}


  {% for semester in all_semesters %}
    <div class="semester-group">
      <details>
      <summary>
      <!-- <h3 class="semester-head"> Display Semester Name -->
      <b>{{ semester }}</b>
      <!-- </h3> -->
      </summary>
      <ul>
      <div class="members-list">
        {% for member in site.members %}
          {% if member.semester contains semester %}
    
                  <!-- Initialize variables for semester tracking -->
                  {% assign active_semesters = "" %}
                  {% assign post_count = 0 %}
                  
                  {% assign status = "예비회원" %} <!-- Default to Fresh -->
                  {% assign stat-emoji = "&#129364;" %}

                  <!-- Loop through the posts to calculate the status -->
                  {% assign member_posts = site.posts | where: "member", member.name %}
                  
                  {% for post in member_posts %}
                    {% for semester in post.semester %}
                      <!-- Count posts for each semester -->
                      {% assign post_count = post_count | plus: 1 %}

                      <!-- If there are more than two posts in the same semester, consider it "active" -->
                      {% if post_count >= 2 %}
                        {% unless active_semesters contains semester %}
                          {% assign active_semesters = active_semesters | append: semester | append: "," %}
                        {% endunless %}
                      {% endif %}
                    {% endfor %}
                  {% endfor %}

                  <!-- Determine status based on post count and semester distribution -->
                  {% assign active_semester_count = active_semesters | split: "," | size %}
                  
                  {% if member_posts.size == 0 %}
                    {% assign status = "예비회원" %}
                    {% assign stat-emoji="&#129364;" %}
                  {% elsif member_posts.size > 0 and active_semester_count == 0 %}
                    {% assign status = "신입회원" %}
                    {% assign stat-emoji="&#127807;" %}
                  {% elsif active_semester_count == 1 %}
                    {% assign status = "활동회원" %}
                    {% assign stat-emoji="&#127795;" %}
                  {% elsif active_semester_count > 1 %}
                    {% assign status = "명예회원" %}
                    {% assign stat-emoji="&#127822;" %}
                  {% endif %}


            <article class="member-item">
              {% if member.rank and member.rank != "" %}
              <li><b><a href="{{ member.url }}">{{ member.name }}</a> {{stat-emoji}}</b></li>
              {% else %}
              <li><a href="{{ member.url }}">{{ member.name }}</a> {{stat-emoji}}</li>
              {% endif %}


            </article>
          {% endif %}
        {% endfor %}
        
      </div>
      </ul>
      </details>
    </div>
  {% endfor %}
</div>
