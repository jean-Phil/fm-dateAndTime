## Date and Time Library of Custom Functions and Scripts for FileMaker


At this point, this file is a prototype of how we could deal with date and time conversion and formatting within FileMaker, but it has the broader aim.

The ideas behind it is to:
- Add a way to add an optional timezone/offset component to the FileMaker timestamp;
- Parse incoming and format outgoing JSON dateTime information ( provide support for the main internet standards (unix epoch, iso8601, RFC 2445, RFC 2822 );
- Provide (a) standard way(s) to express/store repeating events ( support for RFC 2445 RECUR rules);
- Have more control over the localization and display of dates in solutions.
    


## Roadmap

### v0.2
- Test and stabilize the conversion + formatting function
- Add UTC offset output;
- Stabilize offset + timezone inputs
- Add SQL input and output

### v0.3
- Add input conversion and output validation
- "Tokenize" the input parser  à la moment.js , — moment("2010-10-20 4:30", "YYYY-MM-DD HH:mm") —
- Streamline CF parameter passing, ( option a: use JSON object with various options; option b: break main function into smaller, more specific use ones ) 

### v0.4

- Add iCalendar notation RECUR/REPEAT parsing 
- Aim to infer the type of the input string


### v0.5

- Improve documentation
- Add more languages
