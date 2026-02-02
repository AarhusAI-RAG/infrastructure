Jacob wrote:
> Hvis I arbejder med agenter er nuværende SOTA-principper overordnet set:
> Rekursiv nedbrydning af opgaver, opbevar dem i dependency graph som fanges af agent instances (robusthed, parallelisering, kontekstoptimering)
> Ralph Wiggum-princippet (nyt navn, ældre princip): minimér altid kontekst ved at slutte sessions hurtigst muligt (derfor nedbrydning af opgaver) og log state
> Functional tilgang
>  
> Klassisk langgraph mv.-tilgang er mere a la:
> Agent = klasseinstans i react-loop
> OOP tilgang
>  
> Det kan laves med enten Python REPL-stil (det, jeg roder med, fordi det giver fleksibilitet, sparer kontekst og qwen er rimelig god til det), eller mere klassisk function calling sdk (som openai's jeg har brugt til demoen på byrådsdata, som flere modeller er trænede til), eller smolagents som er mere markdown-reasoning fulgt af code-block. Hvis I er mere til standard tool calling, kan I også køre en 4bit kvantiseret gpt-oss-120b på den nye GPU, når den kommer (eller qwen, den skulle også være god til standard tools).
>  
> Anywho, mit eget take for mindre modellers succes er derudover: nedbrydning af "roller" (formål)
>  
> Dvs. i stedet for "hey du er en coder, der skal ræsonnere om hvordan du laver god kode med det her module til den her opgave", så i stedet "hey du er en coder, der løser den her opgave med kode" og "hey du er en reviewer, der sender slamkode retur, hvis ikke det løser den her opgave" og "hey du er en context pruner, der klipper det environment feedback væk, som ikke bidrager til at løse den her opgave" og "hey du er en evaluator der brokker dig hvis den her endelige returværdi ikke dokumenterer at hele opgaven er løst tilfredssstillende" osv.
>  
> Samme princip gælder den gate / planner / manager, der ligger foran.
>  
> Det vigtige er hele tiden tungen lige i munden ift. context. Coderen der har brug for 4 steps for at løse en opgave, skal ikke have med i sin history at den fejlede 3 gange ved step 2, når den først er videre. Det er en context-blindtarm, der bare skal opereres væk.
> 
> Men det er en balance. Test, test, test.
