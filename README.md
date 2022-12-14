# Welcome to my Spells Databse!
This is a database of spells for DND 5e!

It's been an annoyance to me for a long time that 
there aren't any good databases for fifth edition spells. So I decided to take
my preferred dnd resource, the dnd wikidot, and use Rust to scrape the 5th edition
spells. That being said, I scraped *most* of the data, meaning that some parts of
it I had to do by hand, so in particular `proc`, `damage`, and `tags` are highly 
succeptable to errors in formatting. I know for a fact that capitalization is not
consistant, so I highly reccomend something like `.toLower()` to make sure that searches
are proper. As for pull requests and all that: I'm fairly new to github. If you would like
to contribute, please feel free with the understanding that you'll have to bear with me as I 
navigate github. 

## JSON Formatting
```json
{
    "title": "title here",
    "source": "book source here",
    "level": 9999,
    "school": "evocation, conjuration, etc",
    "ritual": true,
    "casting_time": [
        9999, 
        "action/reaction",
        [
        false,
        false,
        false
        ]
    ],
    "component_cost": true,
    "range": [
        9999,
        9999,
        "none/Cube/Sphere"
    ],
    "duration": [
        9999,
        "Instant/Hour/Round",
        false
    ],
    "text": "spell of title does thing, many thing in fact",
    "higher_lv": "At higher levels spell of title does many many thing",
    "spell_lists": [
        "Sorcerer",
        "Warlock",
        "Druid",
        "Cleric"
    ],
    "proc": [
        "Instant/Save/Melee Attack/Ranged Spell Attack",
        "Wisdom/Charisma/Strength"
    ],
    "damage":[
        9999,
        9999,
        9999,
        [
            "Acid",
            "Lightning",
            "Cold"
        ]
    ],
    "tags": [
        "Attack",
        "Healing",
        "Utility",
        "Control",
        "Buff",
        "Debuff"
    ]
}
```
When creating homebrew spells, the two main things to be careful about is the array lengths and keywords. 
While the search is forgiving for capitalization, it cannot properly handle mispelled words, so be careful about keywords such as proc, damage type, and tags. 
Keep in mind while mispelled tags cannot break the program (probably), they will create new entries
The other thing to take note of is which fields have a set size and which can have any size. To add another spell, simply create a new file with a `.json` extension
from the template above, and add it to the applicable folder in the spells/ directory.

#### Specific formatting rules: 
1. `"title": "title here"` This is where the title goes (String)
2. `"source": "book source here"` This is where the book source goes. If you made a homebrew spell, feel free to put your name here, or leave it with an empty string (not empty, but `""`) (String)
3. `"level": 9999` This is the level of the spell, use 0 for cantrip (Number, unsigned 32 for nerds)
4. `"school": "evocation, conjuration, etc"` This is where the school that the spell belongs to. Note that this has multiple schools only for example purposes (String).
5. `"ritual": true/false` This is whether the spell can be cast as a ritual (True or False, bool for nerds),
6. `"casting_time": [ 9999, "action/reaction", [ true/false, true/false, true/false ] ]` These are the rules for the casting time of the spell, are there are a set number of entries.
It makes more sense to explain the second first, it's the time type of the cast, for example "minutes", "hours", "action" (String). 
The first number is the length of the cast. For example *1* round, *5* minutes, *0* Instant. (Number, unsigned 32 for nerds). 
The last entry is a 3 length array representing whether the spell requires V, S, and/or M components (all 3 are True or False, bool for nerds).
7. `"component_cost": true/false` This represents whether the spell requires a physical component that costs money **and is consumed**. Please annotate that component in the `text` (True or False, bool for nerds).
8. `"range": [9999, 9999, "none/Cube/Sphere"]` This represents the range of the spell, and has a set number of entries. 
The first number represents the range of the spell source, measured in feet. 0 for self, 1 for touch. As a rule of thumb, melee spells have a range of 5. (Number, unsigned 32 for nerds).
The second number represents the range of the spell area of effect, measured in feet. (Number, unsigned 32 for nerds).
The last entry represents what kind of area of effect the spell has, from `"none"` to `"Cube"` to even `"Hemisphere"` (String).
9. `"duration": [ 9999, "Instant/Hour/Round", true/false ]` This represents how long the spell lasts for, and has a set number of entries. 
The first element represents how long the spell lasts (Number, unsigned 32 for nerds).
The second element represents what kind of time the first element represents, such as `"Instant"`, `"Rounds"`, `"Minutes"`, etc. (String).
Please note what triggers any reactions in `text`. 
The last element represents whether or not the spell requires concentration (True or False, bool for nerds). 
10. `"text": "spell of title does thing, many thing in fact"` This is contains the body text of the spell (String).
11. `"higher_lv": "At higher levels spell of title does many many thing"` If there is a higher level option for the spell, this goes here. If there is no higher levels, 
it is important to leave this entry as `""`, not empty. (String)
12. `"spell_lists": ["Sorcerer", "Warlock", "Druid", "Cleric"]` This represents what spell lists this spell belong to. 
This is the first list of any size, and can even have no elements *yaaaay*. (All elements are strings)
13. `"proc": ["Instant/Save/Melee Attack/Ranged Spell Attack", "Wisdom/Charisma/Strength"]` This represents how this spell activates, and has a set number of elements. 
The first element is how it activates, whether it is instant, or on a save, or a Ranged Spell Attack (String).
The second element is special for "Save" activations, and holds what skill save type needs to be rolled. 
If the first element isn't a save, then please leave as "none" for sake of consistancy (String).
14. `"damage": [9999, 9999, 9999, ["Acid", "Lightning", "Cold"] ]` This represents what kind of damage the spell deals. This has a set number of elements. 
The first three items can be thought as `X`D`Y` + `Z` (all Numbers, unsigned 32 for nerds). 
The last element is a variable size list, and can even hold no items, and represents what damage type(s) the spell deals (Strings). 
15. `"tags": ["Attack", "Healing", "Utility", "Control", "Buff", "Debuff"]` This represents what tags are attached to this spell and is a list of variable size
, whether it's an attack or it heals or it can be used as utility. Note that temporary hp is usually listed under healing, 
utility is spells that can be used as a exploration/role playing tool (use loosely), and control is usually spells that control the battlefield, 
but can also be spells that influence movement of enemies (Strings).


##### What's next?
I'm going to embark on creating a tui for these spells in rust soon, but first
I will be going into a data analytics program by the name of PowerBI to see 
what I can see. What does that mean for this project? That means that I will be 
able to see if certain things are mispelled, and be able to provide lists of all 
the tags used


### The MIT License (MIT)
#### Copyright (c) 2022 Gerrit15

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
