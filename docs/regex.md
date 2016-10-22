# Regular Expressions

A Unicode-aware regular expression facility is provided as a convenience for concisely defining efficient matching services to operate on strings. A regular expression literal is a special kind of service literal.

    // constant defined as a regex literal
    phoneMatcher is /\d{3}[.-]?\d{3}[.-]?\d{4}/;

    // success returns the match; fails if not found
    found = phoneMatcher(phoneNum);

    // success returns an array of all matches; fails if none found
    all = phoneMatcher(phoneBook, true);
    
    // alternative form takes strings
    nameMatcher = regex("\W\w+ \W\w+");