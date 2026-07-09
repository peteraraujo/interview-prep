### Exact Match Search

* Requires the user's input string to perfectly align with the stored database record, character for character.
* Case sensitivity can be toggled, but typographical errors or missing spaces result in zero returned records.
* Executed on specific identifiers, such as order numbers, database primary keys, or postal codes.

### Fuzzy Search

* Utilizes approximate string matching algorithms (such as the Levenshtein distance) to calculate the mathematical difference between the input string and database records.
* Returns results even if the user introduces typographical errors, transposes letters, or misspells a word.
* Requires higher computational overhead than exact matching due to the calculation of acceptable error thresholds.

### Full-Text Search

* Examines all text within a document or record rather than relying on strictly defined metadata fields.
* Relies on an inverted index, a database structure that maps every unique word in a dataset to the specific documents that contain it.
* Utilizes stemming to reduce words to their root form (e.g., ensuring "running" matches "run") and stop-word removal to ignore common words (e.g., "the," "and") to optimize processing speed.

### Faceted Search

* Combines standard text querying with dynamic categorical filtering.
* After an initial text search retrieves a broad dataset, the system analyzes the metadata of the returned results and generates specific filters (facets) based on those exact results.
* Allows users to systematically narrow a massive data pool by selecting specific data attributes, such as price ranges, dates, or boolean states, updating the result set with each selection.

### Wildcard Search

* Allows users to substitute specific characters in a search string with a placeholder symbol, typically an asterisk (`*`) or percent sign (`%`).
* A trailing wildcard (e.g., `arch*`) matches any record beginning with the specified characters, returning "architecture" and "archaeology."
* Leading wildcards (e.g., `*tecture`) bypass standard index sorting and require the database to scan every string in the specified column, severely degrading query performance.