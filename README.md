*Please do not fork this repository.*

# C# Rocks!

Welcome to the Wizkids C# test!
The test is inspired by one of our products (AppWriter) which helps people with reading/writing problems by enabling them to read text out load and get word suggestions (predictions) while typing.

Predictions are word completions based on the input text, for example:
If the input text is `I like ca` the predictions could be `cars`, `cats` and `cake`.

The test requires you to create a small WPF project that implements the word prediction functionality with results from a custom dictionary and our word prediction web service.

If you encounter any unexpected issues please contact us at `srs AT wizkids.dk` and we will get back to you ASAP.

## Scope

We expect a simply structured C# WPF application with good readability. Design and UX is not a major consideration - keeping it simple is fine.

We expect a well-versed developer to be able to complete the tasks within a few hours.

*While a simple test project you should structure it as a project meant for a business production environment.*

## Code-behind tasks

#### 1. Write a client for the word predictions web service.

#### 2. Write a service for fetching words from the custom dictionary.

## UI/XAML tasks

#### 1. Write XAML code for displaying a text box and two lists of word prediction results (word prediction web service and custom dictionary).

#### 2. Write handlers for fetching word predictions using both the web service client and custom dictionary service.

#### 3. On keystrokes in the text box: Fetch word predictions based on the text value and display the results in the two lists.

- Display the first 10 results from each prediction source.
- The text value used for predictions text should be based on the position of the caret inside the text box, for example:

```
Hello, my name is J|ohn Doe and I like pie! 
```

The extracted text should be from the start of the text box up to the caret, which in this example would be `Hello, my name is J`.

#### 4. Make your word predictions results clickable and insert the clicked prediction into the text box on click.

- The word prediction should be inserted into the text box at the location of the caret.

#### 5. (OPTIONAL) Emphasize the part of each word prediction that has already been written.
For example, using the text from step #3 `Hello, my name is J` the letter `J` should be emphasized in the predictions `John`, `James` and `June`:

> **J**ohn  
> **J**ames  
> **J**une  


## Miscellaneous tasks

#### 1. Generate a token for use with the word predictions web service at https://services.lingapps.dk/misc/CSharpJSRocks.

#### 2. Add your code in a Git project on GitHub and submit the link at https://services.lingapps.dk/misc/CSharpJSRocks.

(OPTIONAL, but strongly recommended) Add a link to one of your C# projects (in which you've been a major contributor) - Git repository preferred.

## Custom dictionary description

The custom dictionary is implemented as an SQLite 3 database (`Dictionary.db`). It contains a `Words`table with the columns `Id` and `Value`.

The database is not encrypted/does not require a password.

If you have issues downloading the database file directly (GitHub some times display a 404 error), try cloning the project instead.


## Word predictions web service description

#### URL
`https://services.lingapps.dk/misc/getPredictions`

#### Authorization
Header: `Authorization: Bearer [TOKEN]` (token generated in Miscellaneous task #1)

#### Parameters
  - `locale`: String, the language in which the predictions should be computed. Valid values are `da-DK` and `en-GB`.
  - `text`: String, the text from which to compute the predictions.

#### Response
JSON encoded array of strings ordered by prediction probability (descending), for example:
`["cars", "cats", "cake"]`

#### Example

**Request**
```
GET /misc/getPredictions?locale=en-GB&text=I%20like%20ca HTTP/1.1
Host: services.lingapps.dk
Authorization: Bearer MjAxOS0wMS0wMQ==.dGVzdEBleGFtcGxlLmNvbQ==.MjExMWMyYjdjZGY3YTU3MmU4MTA5OWY0MDgyMmM0OTk=
Connection: Close

```
**Response**
```
HTTP/1.1 200 OK
Content-Length: 34
Connection: close
Content-Type: application/json

[
    "cars",
    "cats",
    "cake"
]
```
