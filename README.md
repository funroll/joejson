
joejson
=======

What is it?
-----------
joejson is a restriction of json to make it better. For programmers.

However, you can't sum it up by taking the BNF for json and modifying it--it describes how you use it. It's simple, too. 

Spec
----

Here's the full spec for joejson. It's two rules:

1. Whenever you have json in string form, the outer element MUST be a dictionary. You can't have an array at the top level... just add a dictionary with a single key telling you what the array is.
2. All keys in a json dictionary MUST be known at compile time.

Everything below is a hot mess with lots of neat insights in it. But if you follow those two rules, you are using joejson. And those two rules will always be everything that makes up joejson. But keep reading and there's an offer of free beer somewhere.

Tell me more.
-------------

If you have something that you want to put in a dictionary, but those dictionary keys come from a database (or user input) and there can be any number of them, don't use a dictionary. Use an array, and put the desired dictionary keys as a property of each dictionary within the array. This has a few nice effects from the point of view of a developer:

1. Any json object can be stored in a standard dictionary primitive. Always. Most of the reason json libraries have a separate primitive is the top level thing might need to be passed around as an array or a dictionary. If you make the choice be always dictionary, you can always pass a json object around in the language's native dictionary. Your inputs to functions that work with json are always dictionaries. And whenever you dump out json, you always have something telling you what it is, before you get a big list of objects.
2. Any time you see a dictionary in json, you know the keys are something you should load into your brain's short term memory. They're properties of an object. You have, somewhere in your code, an object, that has some sort of property that matches that dictionary value. Or you're just using the dictionary directly with a string. But that value, it always has meaning to you--it's not user input, it's not data. Arrays are data, and dictionaries are code. No data can live on its own, it needs code with it. So everything is a dictionary. Which leads to,
3. You have no data without being able to quickly find the code that uses that data. Also, you can quickly find the data and your eyes can filter out the code. They can look alike, but joejson gives you a trick to tell the difference.

Dictionaries are code, arrays are data. No data without code--you always have at least that one string to tie the chunk of json to everything else it touches.

Frequently asked questions
--------------------------

**How to write it**: joejson, JoeJSON, joe_json, JoeJson--yes. Use the one that is consistent with other things you need to type. joejson is not snooty.

**Version**: nope.

**Name**: So there's lots of json libraries, and most things you think of are taken, if they sounds pretty good. joejson sounds kind of nice and it's easy to type.

**Performance**: Yes! You say, but I like dictionaries. I'm doing many operations, and I need logarithmic search time. So put your data in a tree, or a min-heap, or whatever. But when you serialize to json, you need to convert it back to an array. Which forces you to think about sorting. Which is a good thing.

**Loops**: Performance doesn't matter super much to you, or you wouldn't use json in the first place. Binary structs are faster and smaller. So instead of some things being dictionary lookups, and some things being actual dictionary enumerations, and some things being arrays, you just loop through arrays. So you end up with lots of nested loops. Some people don't like nested loops, but they have a few things. They're easy! Easy to write, easy to read, easy to figure out what they're doing. And if all your iteration of data is looping through arrays, you can get used to working with a few primitives and you don't have to think about them as much. Vs. switching back and forth mentally. 

**More loops**: If you're looping nested and nested and nested, you run out of lateral space for indentation. So you make functions for certain loops. Which forces you to name them. Which helps the people reading your code.

**More performance**: If you see stuff where someone bothered to take stuff out of an array and put it in something else, that has some meaning to you, maybe it tells you that is in an inner loop and he needed to tune it

**But dictionaries!**: I know you like json dictionaries. But joejson says "no!". Here's the thing, you see the data and say "this is the key". Obviously, people will want to look up students by user id. But the next guy says, "well, I'm always searching for them by last name, so I'll make that the key in the dictionary". The data doesn't have a key--just make each of those elements that are known at compile time, they're dictionary keys. What varies at runtime is the lists of these things, and the dictionary values. Not the keys.

**But dictionaries and performance again and I'm a busy guy**: "I can't be copying all this data around and transforming it and stuff, I need to go straight to the dictionary so I don't have it around twice." This is false. Seriously. Protocol buffers has no dictionary construct. Here's part of the reason: a red black tree or similar is often not the best way to do something with your data to make it fast, or memory efficient or whatever. And dictionary lookups hide the looping... there's no braces or indentation to tell you it is happening. And it doesn't get named properly. And the net effect is you use dictionaries because "no not nested loops" and you end up not consciously making the performance decisions. If you make the nested loops, you get all these other benefits... performance measuring is super-simple. Then, when something does slow you down, you choose what to do to fix it. Maybe it's not your dictionary lookup that's the right thing, maybe it's a cache. Or maybe you really need a min-heap or even a circularly linked list or something else. But arrays are not bad. Plus, then it forces you to think about the sort order when they come in. And sort order is important as a programmer.

**Really?**: YES! Seriously, just try it. You WILL love it, once your brain adapts. If you use joejson for a not-tiny project and you don't like it, you can come to Manhattan and I will buy you a beer. As long as it's still 2014.

- funroll (Peter Jenkins) <peter@funroll.co>, August 17, 2014

