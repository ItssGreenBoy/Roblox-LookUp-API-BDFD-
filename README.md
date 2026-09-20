# MAIN COMMAND
trigger: `trigger` or maybe `?user`( thats how i use )
`$httpAddHeader[x-api-key;0dc1774fdb0037cc203525dc91e2e8c1]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/$message]

$if[$httpStatus==200]
$title[$httpResult[displayName] (@$httpResult[username])]
$embeddedURL[$httpResult[profileUrl]]
$description[**Friends:** $httpResult[stats;friends]  •  **Followers:** $httpResult[stats;followers]  •  **Following:** $httpResult[stats;following]]
$addField[ID;$httpResult[id];true]
$addField[Verified;$httpResult[verified];true]
$addField[Inventory;$httpResult[inventory;public];true]
$addField[Created;$httpResult[created;formatted];true]
$addField[Last Online;$httpResult[presence;lastOnlineFormatted];true]
$addField[Badges;$httpResult[badgesText];true]
$thumbnail[$httpResult[avatar;fullBodyUrl]]
$color[#2B2D31]

$newSelectMenu[roblox-menu;1;1;Pick a section...]
$addSelectMenuOption[roblox-menu;User Profile;profile-$httpResult[id];Back to the main profile;no;👤]
$addSelectMenuOption[roblox-menu;Avatar;avatar-$httpResult[id];Full avatar render;no;🧍]
$addSelectMenuOption[roblox-menu;Groups;groups-$httpResult[id];Groups they're in;no;👥]
$addSelectMenuOption[roblox-menu;Games;games-$httpResult[id];Games they've created;no;🎮]
$addSelectMenuOption[roblox-menu;Currently Wearing;wearing-$httpResult[id];Currently equipped items;no;👕]
$addSelectMenuOption[roblox-menu;Previous Usernames;usernames-$httpResult[id];Past usernames;no;📜]
$addSelectMenuOption[roblox-menu;Friends;friends-$httpResult[id];Friends list;no;🤝]
$else
$ephemeral
$description[❌ Couldn't find a Roblox user called `$message`.]
$color[#ED4245]
$endif
$endif`

-----------------------------
$onInteraction[roblox-menu]
$if[$httpResult[hasNext]==true]
$addButton[no;roblox-next;Next ▶;secondary]
$endif
$else
$ephemeral
$removeButtons
$description[❌ $httpResult[error]]
$color[#ED4245]
$endif
$reply
$nomention
$elseif[$var[section]==games]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/id/$var[userId]/games]
$if[$httpStatus==200]
$removeButtons
$title[Games ($httpResult[count]) — page $httpResult[page]/$httpResult[totalPages]]
$description[$httpResult[text]]
$color[#2B2D31]
$setVar[robloxNav;games-$var[userId]-$httpResult[prevPage]-$httpResult[nextPage]]
$if[$httpResult[hasPrev]==true]
$addButton[yes;roblox-prev;◀ Previous;secondary]
$endif
$if[$httpResult[hasNext]==true]
$addButton[no;roblox-next;Next ▶;secondary]
$endif
$else
$ephemeral
$removeButtons
$description[❌ $httpResult[error]]
$color[#ED4245]
$endif
$reply
$nomention
$elseif[$var[section]==wearing]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/id/$var[userId]/wearing]
$if[$httpStatus==200]
$removeButtons
$title[Currently Wearing ($httpResult[count]) — page $httpResult[page]/$httpResult[totalPages]]
$description[$httpResult[text]]
$color[#2B2D31]
$setVar[robloxNav;wearing-$var[userId]-$httpResult[prevPage]-$httpResult[nextPage]]
$if[$httpResult[hasPrev]==true]
$addButton[yes;roblox-prev;◀ Previous;secondary]

----------------------------------------------------
$onInteraction[roblox-next]
$textSplit[$getVar[robloxNav];-]
$var[section;$splitText[1]]
$var[userId;$splitText[2]]
$var[targetPage;$splitText[4]]

$httpAddHeader[x-api-key;0dc1774fdb0037cc203525dc91e2e8c1]
$nomention
$reply
$if[$var[section]==groups]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/id/$var[userId]/groups?page=$var[targetPage]]
$if[$httpStatus==200]
$removeButtons
$title[Groups ($httpResult[count]) — page $httpResult[page]/$httpResult[totalPages]]
$description[$httpResult[text]]
$color[#2B2D31]
$setVar[robloxNav;groups-$var[userId]-$httpResult[prevPage]-$httpResult[nextPage]]
$if[$httpResult[hasPrev]==true]
$addButton[yes;roblox-prev;◀ Previous;secondary]
$endif
$if[$httpResult[hasNext]==true]
$addButton[no;roblox-next;Next ▶;secondary]
$endif
$else
$ephemeral
$removeButtons
$description[❌ $httpResult[error]]
$color[#ED4245]
$endif
$nomention
$reply
$elseif[$var[section]==games]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/id/$var[userId]/games?page=$var[targetPage]]
$if[$httpStatus==200]
$removeButtons
$title[Games ($httpResult[count]) — page $httpResult[page]/$httpResult[totalPages]]
$description[$httpResult[text]]
$color[#2B2D31]
$setVar[robloxNav;games-$var[userId]-$httpResult[prevPage]-$httpResult[nextPage]]
$if[$httpResult[hasPrev]==true]
$addButton[yes;roblox-prev;◀ Previous;secondary]
$endif
$if[$httpResult[hasNext]==true]
$addButton[no;roblox-next;Next ▶;secondary]
$endif
$else
$ephemeral
$removeButtons
$description[❌ $httpResult[error]]
$color[#ED4245]
$endif
$nomention
$reply
$elseif[$var[section]==wearing]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/id/$var[userId]/wearing?page=$var[targetPage]]
$if[$httpStatus==200]
$removeButtons
$title[Currently Wearing ($httpResult[count]) — page $httpResult[page]/$httpResult[totalPages]]
$description[$httpResult[text]]
$color[#2B2D31]
$setVar[robloxNav;wearing-$var[userId]-$httpResult[prevPage]-$httpResult[nextPage]]
$if[$httpResult[hasPrev]==true]
$addButton[yes;roblox-prev;◀ Previous;secondary]
$endif
$if[$httpResult[hasNext]==true]
$addButton[no;roblox-next;Next ▶;secondary]
$endif
$else
$ephemeral
$removeButtons
$description[❌ $httpResult[error]]
$color[#ED4245]
$endif
$nomention
$reply
$elseif[$var[section]==usernames]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/id/$var[userId]/usernames?page=$var[targetPage]]
$if[$httpStatus==200]
$removeButtons
$title[Previous Usernames ($httpResult[count]) — page $httpResult[page]/$httpResult[totalPages]]
$description[$httpResult[text]]
$color[#2B2D31]
$setVar[robloxNav;usernames-$var[userId]-$httpResult[prevPage]-$httpResult[nextPage]]
$if[$httpResult[hasPrev]==true]
$addButton[yes;roblox-prev;◀ Previous;secondary]
$endif
$if[$httpResult[hasNext]==true]
$addButton[no;roblox-next;Next ▶;secondary]
$endif
$else
$ephemeral
$removeButtons
$description[❌ $httpResult[error]]
$color[#ED4245]
$endif
$nomention
$reply
$elseif[$var[section]==friends]
$httpGet[https://roblox-lookup-api.onrender.com/api/user/id/$var[userId]/friends?page=$var[targetPage]]
$if[$httpStatus==200]
$removeButtons
$title[Friends ($httpResult[count]) — page $httpResult[page]/$httpResult[totalPages]]
$description[$httpResult[text]]
$color[#2B2D31]
$setVar[robloxNav;friends-$var[userId]-$httpResult[prevPage]-$httpResult[nextPage]]
$if[$httpResult[hasPrev]==true]
$addButton[yes;roblox-prev;◀ Previous;secondary]
$endif
$if[$httpResult[hasNext]==true]
$addButton[no;roblox-next;Next ▶;secondary]
$endif
$else
$ephemeral
$removeButtons
$description[❌ $httpResult[error]]
$color[#ED4245]
$endif

$endif

