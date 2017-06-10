## Draft/Publish

- Every 'Save' creates a new opinion, and links that opinion with [:THINKS]
- Every 'Publish' creates a new opinion, links that opinion with both [:THINKS] and [:OPINES], and deletes any [:OPINES] from other opinions

Random notes:
- when adding a user to the friend pool, we should check if that friend is already in there
- we need to add email verification; since we're using that as the search mechanism, we shouldn't let people falsely represent others (say: register under donald.trump@gmail.com)
- make queryBuilder relationship syntax consistent (store `:` in relationships.js?)
- we should move pools and everything _not_ related to traversals out of neo4j. All added edges increase the cost of traversals.
