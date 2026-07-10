# Use calls for results and events for reactions

Synchronous public calls are used when a workflow requires an immediate result, while integration
events represent committed facts with independent reactions. Mandating events for every interaction
was rejected because it adds hidden control flow and consistency overhead; unrestricted direct calls
were also rejected because they create cycles without explicit contracts.
