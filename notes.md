Session:
- User
- Topic

World: 
- Session
- Navigation
- Reports

World.Actions:
- SetSessionUser
- SetSessionTopic
- NavigationMsg
- ReportsMsg


## Record Syntax with Opinion

Pros: 

- composable model

Cons:

- need to hookup Displayable and Opinion?
    - could use a convenience method?


## Draft/Publish

- Every 'Save' creates a new opinion, and links that opinion with [:THINKS]
- Every 'Publish' creates a new opinion, links that opinion with both [:THINKS] and [:OPINES], and deletes any [:OPINES] from other opinions


## Union Types

### Backed i a

- Fresh a -- not linked to a db object
- Linked i a -- linked via id (i) to a db object

### Loading e a

- NotRequested
- Requested
- Failed e
- Success a

### Saving a

- Saved a
- Saving a



# Revised Design

## Model

- Current User
- Current Trustees
- Current Topic
- Current Opinion
- Current Author
- Current Author Credentials




