# projred-frontend
Frontend for Project Redistribute.

End-goal of the project:
  - Provide a user friendly, fast way to:
    1. Post donations
    2. Claim donations
    3. Communicate with those who have posted donations
    4. Store/Track donation history information; bounce donation reciepts out to donors and pan inventory levels to recieving centers (RC's)

NOTE: anything *in blue* is something I think might be its own method

Back-End:

1. Store a list of Existing Donors/RC's
  - Donor
    - Name
    - Number(s)
    - Location
    - Hours of pickup
    - Email
    - Password
  - RC
    - Name
    - Number(s)
    - Location(s)
    - Email
    - Password

2. Store a list of existing donations
  - Donor Info: 
    - Name, Number, Location, Hours, Email
  - Item(s)
  - Deadline for pickup
  - Special instructions

3. Store a history of missed donations and confirmed pick-ups
  - Missed Donations
    - Donor
    - Item(s)
    - Date
  - Confirmed pick-ups
    - Donor
    - RC
    - Items
    - Valuation
    - Date

4. Store (temporary) number and message
    - number
    - asking_if_donor boolean
    - most recent message

5. *Handle pick up text*
    - //volunteer sends confirmation text with info on items collected/pans dropped off
    - //info is updated upon reciept of this text

6. *Handle Claim*
    - //RC claims food

7. *Handle Donation*
  - Online
    - //food is posted online
  - Text
    - *checks if *asking_if_donor* (in temporary storage)
      - If not...
        - *Checks if entries match donation format*
          - Entries are correct
            - *Checks number against list of authorized numbers* 
              - Recognizes number
                - *Inserts entries into a new line in list of existing donations* (see above)
                - *Posts new line from list of existing donations to Existing Donations Page*
                - *Notifies all RC's via text and email with entries*
                - Send "thank you for your donation, when someone claims it you'll get notified" message
              - Doesn't recognize number
                - Replies to the text saying number not recognized
                - Asks if the number is a member of an existing donor (lists existing donors)
                - Asks to reply with name of donor as  texted exactly, otherwise respond "no"
                - *Adds number to temporary storage*
                  - Sets asking_if_donor boolean (in temporary storage) to true for that number
          - Entries are incorrect/not recognized
            - Replies to text asking for correct format
      - Else (asking_if donor is true)
        - If entry = "no"
            - Replies with text saying a representative from PR will be with you shortly and directs them to sign up link
            - *Notifies PR_admin number(s)*
            - *Deletes number from temporary storage*
        - If entry matches existing donor
            - *Adds that number to existing donors list of authorized numbers*
            - *Inserts entries into a new line in list of existing donations* (see above)
            - *Posts new line from list of existing donations to Existing Donations Page*
            - *Notifies all RC's via text and email with entries*
            - Send "thank you for your donation, when someone claims it you'll get notified" message
        - Else
            - Replies with "I didn't understand. List donor you're with or reply with 'no'" message
            
Front End:

1. Existing Donations page
  - Display list of existing donations (see above)
    - Sort list in order of deadline, soonest first
    - Claim this donation button
      - Calls *handle claim*
    - Add a donation button
      - Calls *handle donation*

2. User Home Page
  - Donor
    - Info: 
      - Name
      - Number(s)
      - Location
      - Hours of pickup
      - Email
      - Password
    - *Update Info* button
  - Reciepts  
  - Donations to date
      - Total money saved
  - RC
    -Info:
      - Name
      - Number(s)
      - Location(s)
      - Email
      - Password
    - *Update Info* button
    - Donations recieved to date
    - Reciepts

3. About us page
