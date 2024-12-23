schema: 
profile_preview_card:
    name: str
    occupation: str 
    avatar: int (avatar_id) 
    profileId: int

/dashboard (same for both candidate and startup)
    // get
        schema
            reached_count: int
            viewed_count: int
            viewed_history: dict[DateTime, int] 
                    e.g.: {2024-12-01: 10, 2024-12-08: 20, 2024-12-15: 33} 
            connect_request_count: int
            matched_profiles_count: int 
            sample_profiles: list[profile_preview_card] (max_length=4)
        
        BE note:
        - count reached: display 
        - count viewed: scroll down through profile 
            + when clicked "more" -> count as a view 
        - create table view
            + profileId_from
            + profileId_to 
            + timestamp 

/discover
    // get
        schema
            type: array
            max_length: 10 
            items:
                type: profile 
                    common_info:
                        "ProfileID" serial [pk, increment]
                        "UserID" integer [unique, not null]
                        "IsStartup" bool [not null, default:false]
                        "Name" varchar(100) [not null]
                        "Email" varchar(255) [not null]
                        "Industry" varchar(100) [not null]
                        "PhoneNumberID" integer [unique, not null]
                        "CountryID" integer [not null]
                        "City" varchar(100)
                        "LinkedInURL" varchar(255) [unique]
                        "Slogan" varchar(200)
                        "WebsiteLink" varchar(255)
                        "Description" varchar(2000)
                        "Achievement" varchar(2000)
                        "Avatar" varchar(255)               // gravatar

                    startup_info:
                        "CurrentStage" varchar(2000) 

                    candidate_info:
                        "University" varchar(200)
                        "HobbyInterest" varchar(2000)
                        "Gender" gender_type
        
    endpoint
        see_more:
            -> /count_view 
        connect:
            -> /connect
        save:
            -> /save

/revisit (skipped)
    // get 
    type: array
    max_length: 10 
    items:
        type: profile_preview_card 
/saved
    // get 
    type: array
    max_length: 10 
    items:
        type: profile_preview_card 
/viewers
    // get 
    type: array
    max_length: 10 
    items:
        type: profile_preview_card 

/profile/{id}
    // get 
        type: profile
        type: experiences
        type: education 
        type: tag
        BE: get accroding to privacy, return null for non-permitted fields

/getuserprofiles (from current loggedin user ID from JWT)
    // get
    type: array
    items:
        type: profile_preview_card 

/profile/me (current loggedin profile)
    // get
        type: profile
        type: experiences
        type: education 
        type: tag 
        BE: get everything

    // patch 
        type: profile
        type: experiences
        type: education 
        type: tag 
        type: privacy

/profile/create (current loggedin profile)
    // post 
        type: profile
        type: experiences
        type: education 
        type: tag 


/count_view (post)
    from_profileId: int
    to_profileId: int
    timestamp: DateTime

/upload_avatar (post)

/connect (post)
    from_profileId: int
    to_profileId: int
    timestamp: DateTime

/save (post)
    from_profileId: int
    to_profileId: int
    timestamp: DateTime

/view (post)
    from_profileId: int
    to_profileId: int
    timestamp: DateTime

/discover/candidate
/discover/startup

/user/me: view & edit my profile 

