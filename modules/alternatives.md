# Alternative Suggestion Module {#alternatives-suggestion-module}

This module is able to retrieve from its dictionary the entries most similar to the input form. The similarity is computed according to a configurable string edit distance (SED) measure.

The alternatives module can be created to perform a direct search of the form in a dictionary, or either to perform a search of the phonetic transcription of the form in a dictionary of phonetic transcriptions. In the later case, the orthographic forms corresponding to the phonetically similar words are returned. For instance, if a mispelled word such as spid is found, this module will find out that it sounds very close to some correct words in the dictionary (e.g. speed, spit), and return the correctly spelled alternatives. This module is based on the fast search algorithms on FSMs included in the finite-state libray FOMA (<small class="SMALL"><tt>[http://code.google.com/p/foma](http://code.google.com/p/foma)</tt></small>).

The API for this module is the following:

```C++
class alternatives {
  public:
    /// Constructor
    alternatives(const std::wstring &cfgfile);

    /// Destructor
    ~alternatives();

    /// direct access to results of underlying FSM to retrieve
    /// most similar words to the given one
    void get_similar_words(const std::wstring &form,  
                           std::list<std::pair<std::wstring,int> > &results) const;

    /// analyze given sentence.
    void analyze(sentence &s) const;

    /// analyze given sentences.
    void analyze(std::list<sentence> &ls) const;

    /// return analyzed copy of given sentence
    sentence analyze(const sentence &s) const;

    /// return analyzed copy of given sentences
    std::list<sentence> analyze(const std::list<sentence> &ls) const;
};
```

This module will find alternatives for words in the sentences, and enrich them with a list of forms, each with the corresponding SED value. The forms are added to the <tt>alternatives</tt> member of class <tt>word</tt>, which is a <tt>std::list<pair<std::wstring,int»</tt>. The list can be traversed using the iterators <tt>word::alternatives_begin()</tt> and <tt>word::alternatives_end()</tt>.

The constructor of this module expects a configuration file containing the following sections:

*   Section `<General>` contains values for general parameters, expressed in lines of the form `key value`.

    More specifically, it must contain a line `Type (orthographic|phonetic)` stating whether the similar words must be searched using direct SED between of orthographic forms, or between their phonetic encoding.

    This section must also contain one line `Dictionary filename` or either a line `PhoneticDictionary filename` stating the dictionary where the similar words are going to be searched. If `PhoneticDictionary` is stated, then an additional line `PhoneticRule filename` is expected, detailing the configuration file for a phonetic encoding module (see section 

    4.18

    ) that will be used to encode the input forms before the search.

The `Dictionary` can be any file containing one form per line. Only first field in the line will be considered, which makes it possible to use a basic FreeLing dictionary (see section 

4.9

), since the morphological information will be ignored.

The `PhoneticDictionary` must contain one phonetic form per line, followed by a list of orthographic forms mapping to that sound. E.g., valid lines are:

<pre>    f@Ur fore four
    tu too two
</pre>

*   Section `<Distance>` contains `key value` lines stating parameters related to the SED measure to use.

    A line `CostMatrix filename` is expected, stating the file where the SED cost matrix to be used. The `CostMatrix` file must comply with FOMA requirements for cost matrices (see FOMA documentation, or examples provided in `data/common/alternatives` in FreeLing tarball).

    A line `Threshold int-value` can be provided stating the maximum distance of desired alternatives. Note that a very high value will cause the module to produce a long list of similar words, and a too low value may result in no similar forms found.

    A line `MaxSizeDiff int-value` may also be provided. Similar strings with a length difference greater than this parameter will be filtered out of the result. To deactivate this feature, just set the value to a large number (e.g. 99).

    *   Section `<Target>` contains `key value` lines describing which words in the sentence must be checked for similar forms.

    The line `UnknownWords (yes|no)` states whether similar forms are to be searched for unknown words (i.e. words that didn't receive any analysis from any module so far).

    The line `KnownWords regular-expression` states which words with analysis have to be checked. The regular expression is matched against the PoS tag of the words. If the regular-expression is `none`, no known word is checked for similar forms.
