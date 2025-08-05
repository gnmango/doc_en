# Course Introduction
Regular expressions use patterns you input to match and find text. This course walks you through the specific categories of characters, operators, and delimiters you can use when defining a regular expression.

# Character Classes
Character classes match a character from a specific set.
|	Character Classes	|	Desc	|	Patterns	|	Matches	|
|	--	|	--	|	--	|	--	|
|	`[`*character_group*`]`	|	Finds matches for every single character within *character_group*. By default, it is case-sensitive.	|	`[ae]`	|	`"a"` in `"gray"`; `"a"`, `"e"` in <br>`"lane"`	|
|	`[^`*character_group*`]`	|	Finds mismatches for every single character within *character_group*. By default, it is case-sensitive.	|	`[^aei]`	|	`"r"`, `"g"`, `"n"` in `"reign"`	|
|	`[`*first*`-`*last*`]`	|	Finds matches for every single character within the range from the *first* to the *last*.	|	`[A-Z]`	|	`"A"`, `"B"` in `"AB123"`	|
|	`.`	|	`\nFinds matches for every single character except .`<br>To find the literal period character (or \u002E), you must precede it with an escape character (\\.).	|	`a.e`	|	`"ave"` in `"nave"`<br>`"ate"` in `"water"`	|
|	`\\p{`*name*`}`	|	Finds matches for every single character in a named block, specified as a Unicode generic category or *name*.	|	`\\p{Lu}`<br>`\\p{IsCyrillic}` |	`"C"`, `"L"` in `"City Lights"` <br>`"Д"`, `"Ж"` in `"ДЖem"`	|
|	`\\P{`*name*`}`	|	Finds mismatches for every single character in a named block, specified as a Unicode generic category or *name*.	|	`\\P{Lu}`<br>`\\P{IsCyrillic}`	|	`"i"`, `"t"`, `"y"` in `"City"` <br>`"e"`, `"m"` in `"ДЖem"`	|
|	`\\w`	|	Finds word characters.	|	`\\w`	|	`"I"`, `"D"`, `"A"`, `"1"`, and `"3"` in `"ID A1.3"`	|
|	`\\W`	|	Finds non-word characters.	|	`\\W`	|	`" "`, `"."` in `"ID A1.3"`	|
|	`\\s`	|	Finds whitespace characters.	|	`\\w\\s`	|	`"D "` in `"ID A1.3"`	|
|	`\\S`	|	Finds non-whitespace characters.	|	`\\s\\S`	|	`" _"` in `"int __ctr"`	|
|	`\\d`	|	Finds decimal numbers.	|	`\\d`	|	`"4"` in `"4 = IV"`	|
|	`\\D`	|	Matches a non-decimal character.	|	`\\D`	|	`" "`, `"="`, `" "`, `"I"`, and `"V"` in `"4 = IV"`	|

# Character Escapes
Character Escapes mean that characters following the backslash character (\\) in a regular expression should be interpreted as special characters or literals, as shown in the table below.
|	Escaped characters	|	Desc	|	Patterns	|	Matches	|
|	--	|	--	|	--	|	--	|
|	`\t`	|	Finds tab character \u0009.	|	`(\\w+)\t`	|	`"item1\t"`, `"item2\t"` in `"item1\titem2\t"`	|
|	`\r`	|	Finds \u000D, the carriage return character. <br>(`\r` is different from `\n`, the newline character).	|	`\r\n(\\w+)`	|	`"\r\nThese"` in `"\r\nThese are\ntwo lines."`	|
|	`\v`	|	Finds vertical tab character \u000B.	|	`[\v]{2,}`	|	`"\v\v\v"` in `"\v\v\v"`	|
|	`\n`	|	Finds line break character \u000A.	|	`\r\n(\\w+)`	|	`"\r\nThese"` in `"\r\nThese are\ntwo lines."`	|
|	`\\`*Nnn*	|	Specifies the characters using an octal expression. <br>(*nnn* is configured with two or three digits).	|	`\\w\\040\\w`	|	`"a b"`, `"c d"` in `"a bc d"`	|
|	`\\x`*Nn*	|	Specifies the characters using a hexadecimal expression. <br>(*nn* is configured with two digits exactly.)	|	`\\w\\x20\\w`	|	`"a b"`, `"c d"` in `"a bc d"`	|
|	`\\u`*Nnnn*	|	Finds the Unicode characters using their hexadecimal expression (*nnnn*, which is configured with exactly four digits).	|	`\\w\\u0020\\w`	|	`"a b"`, `"c d"` in `"a bc d"`	|
|	`\\`	|	Finds a character that is not recognized as an escaped character in this table or any other table in this topic, if found. For example, `\*` is the same as `\x2A`, and `\.` the same as `\x2E`. This allows the regular expression engine to distinguish between linguistic elements such as * or ? and character literals represented by `\*` or `\?`.	|	`\\d+[\\+-x\\*]\\d+`	|	`"2+2"` and `"3*9"` in `"(2+2) * 3*9"`	|

# Anchors
Anchors or atomic zero-width assertions specify where to look for a match in the string. The anchors enable the regular expression engine to search for matches only at the specified locations, without passing through the string or using characters. For example, the `^` symbol allows it to search for the matches at the start of a line or string. `^http:` looks for "http:" only at the start of a line.
|	Anchors	|	Desc	|	Patterns	|	Matches	|
|	--	|	--	|	--	|	--	|
|	`^`	|	Finds the matches at the start of a string. If multiline is enabled, looks for matches at the start of each line.	|	`^\\d{3}`	|	`"901"` in `"901-333-"`	|
|	`$`	|	Finds matches at the end of a string or before `\n` at the end of a string. If multiline is enabled, looks for matches at the end of the line or before `\n` at the end of a string.	|	`-\\d{3}$`	|	`"-333"` in `"-901-333"`	|
|	`\\A`	|	The matches must be only at the start of a string. (Not supporting multiple lines)	|	`\\A\\d{3}`	|	`"901"` in `"901-333-"`	|
|	`\\Z`	|	The matches must be at the end of a string or before the `\n` at the end of a string.	|	`-\\d{3}\\Z`	|	`"-333"` in `"-901-333"`	|
|	`\\z`	|	The matches must be only at the end of a string.	|	`-\\d{3}\\z`	|	`"-333"` in `"-901-333"`	|
|	`\\G`	|	Finds a match at the point where the previous match stops. If there is no previous match, it finds a new match at the position in the string where the new match starts. 	|	`\\G\\(\\d\\)`	|	`"(1)"`, `"(3)"`, and`"(5)"` in `"(1)(3)(5)[7](9)"`	|
|	`\\b`	|	Finds matches on the boundary between a word character `\w` and a non-word character `\W`. |	`\\b\\w+\\s\\w+\\b`	|	`"them theme"`, `"them them"` in `"them theme them them"`	|
|	`\\B`	|	Specifies that matches should not be found on the boundary between the characters. It performs the opposite operation with the `\\b` anchor.	|	`\\Bend\\w*\\b`	|	`"ends"`, `"ender"` in `"end sends endure lender"`	|

# Grouping Constructs
Grouping constructs represent the subexpression of a regular expression and captures a substring of the origin string. 
|	Grouping Constructs	|	Desc	|	Patterns	|	Matches	|
|	--	|	--	|	--	|	--	|
|	`(`*subexpression*`)`	|	Captures a matching subexpression and assigns the ordinal numbers (starting with 2).	|	`(\\w)`	|	`"d"`, `"e"`, `"e"`, `"p"` of `"deep"`	|
|	`(?<`*name*`>`*subexpression*`)` or `(?'`*name*`'`*subexpression*`)`	|	Captures a matching subexpression into the named group.	|	`(?<double>\\w)\\k<double>`	|	`"ee"` in `"deep"`	|
|	`(?<`*name1*`-`*name2*`>`*subexpression*`)` or `(?'`*name1*`-`*name2*`'`*subexpression*`)`	|	Defines a balancing group. <br>In the format of the grouping construct, *name1* is the current group (optional), *name2* is a previously defined group, and *subexpression* is any valid regular expression pattern. The definition of balancing group deletes the definition of *name2* and saves the spacing between the *name1* and the *name2* in *name1*.	|	`(((?'Open'\\()[^\\(\\)]*)+((?'Close-Open'\\))[^\\(\\)]*)+)*(?(Open)(?!))$`	|	`"((1-3)*(3-1))"` <br>in `"3+2^((1-3)*(3-1))"`	|
|	`(?:`*subexpression*`)`	|	Defines a non-capturing group.	|	`Write(?:Line)?`	|	<br>`"WriteLine"` in `"Console.WriteLine()"` <br><br>`"Write"` in `"Console.Write(value)"`	|
|	`(?=`*subexpression*`)`	| Looks for matching patterns <br>before the Positive Lookahead *subexpression*. |	`\\b\\w+\\b(?=.+and.+)`	|	`"cats"`, `"dogs"` in `"cats, dogs <br>and some mice."`	|
|	`(?!`*subexpression*`)`	|	Looks for mismatched patterns <br>before the Negative Lookahead *subexpression*.	|	`\b\w+\b(?!.+and.+)`	|	`"and"`, `"some"`, `"mice"` in `"cats, dogs <br>and some mice."`	|
|	`(?<=`*subexpression*`)`	|	Looks for matching patterns <br>after the Positive Lookbehind *subexpression*.	|	`\\b\\w+\\b(?<=.+and.+)`<br>———————————<br>`\\b\\w+\\b(?<=.+and.*)`	|	`"some"`, `"mice"` in `"cats, dogs <br>and some mice."`<br>————————————<br>`"and"`, `"some"`, `"mice"` in `"cats, dogs <br>and some mice."`	|
|	`(?<!`*subexpression*`)`	|	Looks for mismatched patterns <br>after the Negative Lookbehind *subexpression*.	|	`\\b\\w+\\b(?<!.+and.+)`<br>———————————<br>`\\b\\w+\\b(?<!.+and.*)`	|	`"cats"`, `"dogs"`, `"and"` in `"cats, dogs <br>and some mice."`<br>————————————<br>`"cats"`, `"dogs"` in `"cats, dogs <br>and some mice."`	|
|	`(?>`*subexpression*`)`	|	Atomic group. |	`(?>a\|ab)c`	|	`"ac"` in `"ac"` <br>Not found in `"abc"`	|

# Quantifiers
Quantifiers specify the number of instances of characters, groups, or character classes to find in matches.
|	Quantifiers	|	Desc	|	Patterns	|	Matches	|
|	--	|	--	|	--	|	--	|
|	`*`	|	Finds 0 or more transfer elements.	|	`a.*c`	|	`"abcbc"` in `"abcbc"`	|
|	`+`	|	Finds 1 or more transfer elements.	|	`be+`	|	`"bee"` in `"been"`, `"be"` in `"bent"`	|
|	`?`	|	Finds 0 or 1 transfer element.	|	`rai?`	|	`"rai"` in `"rain"`	|
|	`{`*N*`}`	|	Finds transfer elements exactly *n* times.	|	`,\\d{3}`	|	`",043"` in `"1,043.6"`, <br>`",876"`, `",543"` and `",210"` in `"9,876,543,210"`	|
|	`{`*N*`,}`	|	Finds transfer elements at least *n* times.	|	`\\d{2,}`	|	`"166"`, `"29"`, `"1930"`	|
|	`{`*N*`,`*M*`}`	|	Finds transfer elements at least *n* times, but not more than *m* times.	|	`\\d{3,5}`	|	`"166"`, `"17668"`<br>`"19302"` in `"193024"`	|
|	`*?`	|	Finds at least 0 or as few transfer elements as possible.	|	`a.*?c`	|	`"abc"` in `"abcbc"`	|
|	`+?`	|	Finds at least 1 or as few transfer elements as possible.	|	`be+?`	|	`"be"` in `"been"`, `"be"` in `"bent"`	|
|	`??`	|	Finds 0 or 1 transfer element, or as few as possible.	|	`rai??`	|	`"ra"` in `"rain"`	|
|	`{`*N*`}?`	|	Finds transfer elements exactly *n* times.	|	`,\\d{3}?`	|	`",043"` in `"1,043.6"`, <br>`",876"`, `",543"` and `",210"` in `"9,876,543,210"`	|
|	`{`*N*`,}?`	|	Finds as few transfer elements as possible, at least *n* times.	|	`\\d{2,}?`	|	`"166"`, `"29"`, `"1930"`	|
|	`{`*N*`,`*M*`}?`	|	Finds as few transfer elements as possible, at least *n* to *m* times.	|	`\\d{3,5}?` 	|	`"166"`, `"17668"`<br>`"193"`, `"024"` in `"193024"`	|

# Backreference Constructs
Backreference Constructs are useful for identifying repeated characters or substrings within a string.
|	Backreference Constructs	|	Desc	|	Patterns	|	Matches	|
|	--	|	--	|	--	|	--	|
|	`\\k<`*name*`>`	|	Named backreference constructs. It finds the value of a named expression.	|	`(?<char>\\w)\\k<char>`	|	`"ee"` in `"seek"`	|

# Alternation Constructs
Alternation Constructs modify the regular expressions to allow 'either/or' conditional matching.
|	Alternation Constructs	|	Desc	|	Patterns	|	Matches	|
|	--	|	--	|	--	|	--	|
|	`\|`	|	Matches one element separated by a vertical bar (\|).	|	`th(e\|is\|at)`	|	`"the"`, `"this"` in `"this is the day."`	|
|	`(?(`*expression*`)`*Yes*`\|`*No*`)` or<br>`(?(`*expression*`)`*Yes*`)` | Matches *Yes* if the regular expression pattern specified by the *expression* is matched. Otherwise, it matches *No* part. The *expression* is interpreted as an assertion with zero width.<br>To avoid ambiguity with named or numbered capturing groups, if necessary, you can use explicit assertions as follows:<br>`(?( (?=`*expression*`) )`*Yes*`\|`*No*`)`	|	`(?(A)A\\d{2}\\b\|\\b\\d{3}\\b)`	|	`"A10"`, `"910"` <br>in `"A10 C103 910"`	|
|	`(?(`*name*`)`*Yes*`\|`*No*`)`or<br>`(?(`*name*`)`*Yes*`)` |	Matches *Yes* if there is a match for *name* as a named or numbered capturing group. Otherwise, it matches *No*.	|	`(?<quoted>\")?(?(quoted).+?\"\|\\S+\\s)`	|	`"Dogs.jpg "`, `"\"Yiska playing.jpg\""` <br>in `"Dogs.jpg \"Yiska playing.jpg\""`	|

# Substitutions
Substitutions interact with replacement patterns. They define **all or part of a substitute text for the matching text in the origin string**.
|	Characters	|	Desc	|	Patterns	|	Replacement Pattern	|	Origin string	|	Result string	|
|	--	|	--	|	--	|	--	|	--	|	--	|
|	`${`*name*`}`	|	Substitutes the partial string that matches the named group.	|	`\\b(?<word1>\\w+)(\\s)(?<word2>\\w+)\\b`	|	`${word2} ${word1}`	|	`"one two"`	|	`"two one"`	|
|	`$$`	|	Substitutes "$" literal.	|	`\\b(\\d+)\\s?USD`	|	`$$$1`	|	`"103 USD"`	|	`"$103"`	|
|	`$&`	|	Substitutes a copy of the entire string that matches.	|	`\\$?\\d*\\.?\\d+`	|	`**$&**`	|	`"$1.30"`	|	`"**$1.30**"`	|
|	`$`\`	|	Substitutes all the text in the origin string before the matching string.	|	`B+`	|	`$`\`	|	`"AABBCC"`	|	`"AAAACC"`	|
|	`$'`	|	Substitutes all the text in the origin string after the matching string.	|	`B+`	|	`$'`	|	`"AABBCC"`	|	`"AACCCC"`	|
|	`$+`	|	Substitutes the last group captured.	|	`B+(C+)`	|	`$+`	|	`"AABBCCDD"`	|	`"AACCDD"`	|
|	`$_`	|	Substitutes the entire origin string.	|	`B+`	|	`$_`	|	`"AABBCC"`	|	`"AAAABBCCCC"`	|