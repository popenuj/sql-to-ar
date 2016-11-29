```ruby

SQL to AR

SELECT *
FROM
  users;

------------
User.all
------------

SELECT *
FROM
  users
WHERE
  user.id = 1;

------------
User.where("id = 1")
User.where(id: 1)
------------

SELECT *
FROM
  posts
ORDER BY
  created_at DESC
LIMIT 1;

------------
Post.order(created_at :desc).limit(1)
Post.last
------------

SELECT *
FROM
  users
JOIN
  posts
ON
  posts.author_id = users.id
WHERE
  posts.created_at >= '2014-08-31 00:00:00';

------------
User.joins("JOIN posts ON users.id = posts.author_id").where("created_at >= 2014-08-31 00:00:00")
------------

SELECT
  count(*)
FROM
  users
GROUP BY
  favorite_color
HAVING
  count(*) > 1;

------------
User.select("count(*) AS user_count,").group(:favorite_color).having("user_count > 1")
------------

* The most recently updated user

------------
User.order(created_at: :desc).limit(1)
------------

* The oldest user (by age)

------------
User.order(age: :desc).limit(1)
------------

* all the users

------------
User.all
------------

* all posts sorted in descending order by date created

------------
Post.order(created_at: :desc)
------------

AR to SQL

Post.all

------------
SELECT *
FROM posts;
------------

Post.first

------------
SELECT *
FROM posts
ORDER BY created_at
LIMIT 1;
------------

Post.last

------------
SELECT *
FROM posts
ORDER BY created_at DESC
LIMIT 1;
------------

Post.where(:id => 4)

------------
SELECT *
FROM posts
WHERE id = 4;
------------

Post.find(4)

------------
SELECT *
FROM posts
WHERE id = 4;
------------

User.count

------------
SELECT COUNT(*)
FROM users;
------------

Post.select(:name).where(:created_at > 3.days.ago).order(:created_at)

------------
SELECT name
FROM posts
WHERE created_at > 2016-11-23 00:00:00;
ORDER BY created_at;
# How would use the date without hardcoding?
------------

Post.select("COUNT(*)").group(:category_id)

------------
SELECT COUNT(*)
FROM posts
GROUP BY category_id;
------------

All posts created before 2014

------------
SELECT *
FROM posts
WHERE created_at < 2014-1-1 00:00:00;
------------

A list of all (unique) first names for authors who have written at least 3 posts

------------
SELECT users.name, COUNT(*) AS post_count
FROM posts JOIN users ON posts.author_id = users.id
GROUP BY users.name
HAVING post_count > 2;
------------

The posts with titles that start with the word "The"

------------
SELECT *
FROM posts
WHERE title LIKE 'The%';
------------

Posts with IDs of 3,5,7, and 9

------------
SELECT *
FROM posts
WHERE id IN (3,5,7,9);
------------
