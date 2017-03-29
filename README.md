## Date and Time Library of Custom Functions and Scripts for FileMaker


At this point, this file is a prototype, a proposed way to deal with date&time conversion and formatting within FileMaker.  However, it has the broader aim. The ideas behind it are to:
- Add an optional timezone/offset, as well as a millisecond component to the FileMaker timestamp;
- Parse incoming and format outgoing JSON dateTime information ( provide support for the main internet standards (unix epoch, iso8601, RFC 2445, RFC 2822 );
- Provide (a) standard way(s) to express/store repeating events ( support for RFC 2445 RECUR rules);
- Have more control over the localization and display of dates in solutions.
    

## version 0.1 Release Notes

All inputStrings are converted into a JSON "dateTimeObject", which stores two elements the native FileMaker Timestamp does not currently support, ie. **milliseconds** and **UTC offset**.

```json
{   "year":1997
    , "month":7
    , "day":14
    , "hours":13
    , "minutes":30
    , "seconds":0
    , "milliseconds":null
    , "offset":{ 
        "hours":-5
        , "minutes":0 
        } 
}
```


This transformation is achieved via two "support" custom functions: 
- **z_dateTimeSupport__convertToDateTimeObject**, and
- **z_dateTimeSupport__convertTimezoneToOffset**



The **z_dateTimeSupport__formatDTObject.fromFormatString** custom function takes the generated "dateTimeObjects" and applies formatting, using a token approach.  

```
/* --- the formatting function takes 3 arguments... --- */
z_dateTimeSupport__formatDTObject.fromFormatString ( 
    _dateTimeObject ; 
    _formatString ; 
    _language     /* [optional] -- can default to any local */
)

/* --- passed actual data --- */

z_dateTimeSupport__formatDTObject.fromFormatString ( 
    { "year":2017,"month":8,"day":4,"hours":18,"minutes":0,"seconds":0,"milliseconds":0,"offset":{} } ; 
    "%-m/%-d/%Y %-I:%M %p" ; 
    "" 
) 

// result>  "8/4/2017 6:00 PM"

```



This first version contains a simple localization mechanism.  The file is meant to make extending the functions easy. Adding new languages can easily be done via the i18n table. ( More on how to proceed in later version ). The function responsible for localization is called **z_dateTimeSupport__i18n_format**. 


Finally, a first version of a function which both converts and formats is provided, but it has not been tested yet.  It is meant to give the idea where we are going with all that. The following function call takes a FileMaker timestamp and localizes it to French:

```
/* dateTime.convert ( 
    _inputString ; 
    _inputFormat ; 
    _outputFormat ; 
    _language )
 */

dateTime.convert ( 
     GetAsTimestamp( "1975-05-01 10:00 PM" ) ; 
     "FileMaker Timestamp" ; 
     "%-@d %B %Y, %-H:%M"  ;
     "fr" )

// result> "1er mai 1975, 22:00"
```





## Roadmap

### v0.2
- Test and stabilize the conversion + formatting function
- Add UTC offset output;
- Stabilize offset + timezone inputs
- Add SQL input and output
- Revisit Epoch input and output formula ( I don't think it is correct )
- Make iso8601 and iCalendar more robust

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
