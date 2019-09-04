
### SQL-Challenge

#### 1. Sling TV is an online live tv streaming service provider. It provide 3 basic subscriptions and also users can add more additional services like sports or news add on's. 
#### 2. I was asked to create business metrics to answer few business questions. These business metrics are provided as below.


# How can I tell if someone is viewing shows that they shouldn’t have access to?
Research: I looked into Sling.tv on how user can subscribe to packages. There are base packages like Sling Orange, Sling Blue and Orange & Blue, and users can add more extras to their packages.

SELECT DISTINCT Users.id FROM Users LEFT JOIN Session ON Users.id = Session.users_id AND Users.entitlement = Session.entitlement WHERE Users.active = 'FALSE' AND Session.watched_time > 0 AND Users.updated_at < = session.date_pulled

#### Assumptions:

- Each user can be subscribed to multiple entitlement.

#### Who could be the stakeholders?

- Team handling or working on entitlements.
- Quality testing teams.

#### Graphs/Charts:

1. Pie chart to display the proportion of users viewing the shows with and without active status
2. List displaying all the user Id’s who are watching the show without access

# How many customers viewed a show, I want to be able to filter by a schedule_guid?

SELECT Viewership.schedule_guid, Viewership.title, COUNT(Session.users_id) AS 'Total Users Viewed' 
FROM Viewership 
LEFT JOIN Session 
ON Viewership.asset_id = Session.asset_id 
AND Viewership.date_pulled = Session.date_pulled 
AND Viewership.incoming_host = Session.incoming_host 
WHERE Session.watched_time > 0 
GROUP BY Viewership.schedule_guid, Viewership.title

#### Assumptions:

- Considering all the customers who viewed the show, without taking into account the time duration they watched the show.
- Each show is aired only once in a day.
- schedule_guid is unique key in the table.

#### Who could be the stakeholders?

- UX team - This information can be used to improve user experience by displaying the most viewed shows on the top.
- Marketing team— Helps to display Ads or promotional events.

#### Graphs/Charts
1. Provide filter with a list of schedule_guid. When a schedule guid is selected from the filter a line chart is displayed.
2. Line chart is displayed with the shows and count of total number users who watched each show. This helps to see check any immediate peaks or trends in viewers.
3. Display a heat map showing title and the count for the selected schedule_guid.

# Most viewed and most subscribed entitlements?

#### MOST VIEWED ENTITLEMENTS

SELECT TOP 10 entitlement, COUNT(users_id)
FROM SESSION WHERE watched_time >0 GROUP BY entitlement ORDER BY count(id) DESC

#### MOST SUBSCRIBED ENTITLEMENTS

SELECT TOP 10 entitlement, COUNT(id)
FROM Users WHERE active = 'TRUE' GROUP BY entitlement ORDER BY count(id) DESC

#### Who could be the stakeholders?

- Product & UX team – Helps planning new feature in the road map.
- Marketing & Sales team – Helps them to market these shows and entitlements to new and existing user, and improve user engagement times.

#### Graphs/Charts
1. Display the top 10 most viewed and most subscribed entitlements in bar plot.

# Any insight you think would be beneficial to me?

- Missing values need to be addressed to know if the user is Active or not in User table

- User information and subscription information must be in 2 different tables.

- Table need to be normalized to reduce redundancy.

- Asset related information like duration and title should be created in a separate table and only primary key column of asset table should be included in Viewership and Session tables.

- Genre information can help us to know patterns about viewers’ interests. This information can be used to create new entitlements and also provide recommendation to old customers.
