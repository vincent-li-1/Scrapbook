# Requirements and Specification Document

### Project Abstract

Scrapbook is a photo storage and sharing app that simulates a virtual "scrapbook." Users can upload and categorize photos. They can add "friends" who can view their photos and whose photos they can view, but unlike a standard photo sharing app like Instagram, the purpose of Scrapbook is to encourage users to seek out each others' content. There is no "feed" and no notifications of new posts or engagement, nor the standard social media features of likes and comments. The emphasis is on recording your own photos and memories that others can view as they like.

### User Requirements

- Users can upload their own photos
   - Photos can be categorized by time period or by a custom "label" for given events, people, time periods etc.
        - Custom labels can be grouped to change how a user's scrapbook is portrayed
    - Users can customize how their scrapbook is displayed (photo style, layout style etc) from a set of options
    - Users can set a "starting point" for their scrapbook within any of their custom labels/label groups
- Users can friend other users
    - This allows them to view their friends' scrapbooks, and their friends can view their scrapbooks
        - Scrapbooks are displayed based off of how the owner chooses to display them - the view cannot be customized by other users
    - Users can engage each other in an individual or group private DM, either about a specific photo or group, or just generally
    - There is no feed of friends' photos. Users can go to a friends page where their friends' scrapbooks are accessible from a "coffee table", and go through any given friends' scrapbook.

### User Interface Requirements

Images can be included with `![alt_text](image_path)`

### Security Requirements

Users must log in to their own account using some sort of other SSO - Google, Instagram etc. For security reasons, the initial version of the app will not have its own homebuilt login.

### System Requirements

The app can load a scrapbook in reasonable time (a set of at least 10 images at a time). A user needs to be able to store at least 10,000 photos in their account. The system will initially only be available in web browser, but will eventually be available for mobile browser and as a mobile app

### Specification

<!--A detailed specification of the system. UML, or other diagrams, such as finite automata, or other appropriate specification formalisms, are encouraged over natural language.-->

<!--Include sections, for example, illustrating the database architecture (with, for example, an ERD).-->

<!--Included below are some sample diagrams, including some example tech stack diagrams.-->


#### Technology Stack

```mermaid
flowchart RL
subgraph Front End
	A(Javascript: React)
end
	
subgraph Back End
	B(Javascript: Node.js with Express)
end
	
subgraph Database
	C[(MySQL)]
end

A <-->|"REST API"| B
B <-->|mysql| C
```

#### Database

```mermaid
---
title: Database ERD for Scrapbook
---
erDiagram
    User ||--o{ Post : "created by"
    User ||--o{ Label : "created by"
    Post ||--o{ Label : "describes"

    User {
        int user_id PK
        string user_username
        string user_name
        json settings
        set friends
    }

    Post {
        int post_id PK
        int user_poster_id FK
        int label_id FK
        string post_img_url
        date post_date
        date photo_date
    }

    Label {
        int label_id PK
        int user_id FK
        string group
    }
```

#### Class Diagram

```mermaid
---
title: Class Diagram for Scrapbook
---
classDiagram
    class User {
        - String username
        - Int userId
        - String name
        - Setting userSettings
        - String[] friends
        + User(String username, String name)
        + void setUsername(String username)
        + String getUsername()
        + void setName(String name)
        + String getName()
        + void setSettings(String settingToChange, String change)
        + Setting getSettings
        + void addFriend(String friend)
        + void removeFriend(String friend)
        + String[] getFriends()
    }
    class Settings {
        - User user
        + Settings()
        + void setSetting(String setting, String change)
    }
    
    User <|-- Setting
    Setting <|-- User
```

#### Flowchart

```mermaid
---
title: Sample Program Flowchart
---
graph TD;
    Start([Start]) --> Input_Data[/Input Data/];
    Input_Data --> Process_Data[Process Data];
    Process_Data --> Validate_Data{Validate Data};
    Validate_Data -->|Valid| Process_Valid_Data[Process Valid Data];
    Validate_Data -->|Invalid| Error_Message[/Error Message/];
    Process_Valid_Data --> Analyze_Data[Analyze Data];
    Analyze_Data --> Generate_Output[Generate Output];
    Generate_Output --> Display_Output[/Display Output/];
    Display_Output --> End([End]);
    Error_Message --> End;
```

#### Behavior

```mermaid
---
title: Sample State Diagram For Coffee Application
---
stateDiagram
    [*] --> Ready
    Ready --> Brewing : Start Brewing
    Brewing --> Ready : Brew Complete
    Brewing --> WaterLowError : Water Low
    WaterLowError --> Ready : Refill Water
    Brewing --> BeansLowError : Beans Low
    BeansLowError --> Ready : Refill Beans
```

#### Sequence Diagram

```mermaid
sequenceDiagram

participant ReactFrontend
participant DjangoBackend
participant MySQLDatabase

ReactFrontend ->> DjangoBackend: HTTP Request (e.g., GET /api/data)
activate DjangoBackend

DjangoBackend ->> MySQLDatabase: Query (e.g., SELECT * FROM data_table)
activate MySQLDatabase

MySQLDatabase -->> DjangoBackend: Result Set
deactivate MySQLDatabase

DjangoBackend -->> ReactFrontend: JSON Response
deactivate DjangoBackend
```

### Standards & Conventions

<!--Here you can document your coding standards and conventions. This includes decisions about naming, style guides, etc.-->
