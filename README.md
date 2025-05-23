# Polls API

Polls is a simple API allowing consumers to view polls and vote in them.

## API Root [/]

This resource does not have any attributes. Instead it offers the initial
API affordances in the form of the links in the JSON body.

It is recommended to follow the “url” link values,
[Link](https://tools.ietf.org/html/rfc5988) or Location headers where
applicable to retrieve resources. Instead of constructing your own URLs,
to keep your client decoupled from implementation details.

### Retrieve the Entry Point [GET /]

+ Response 200 (application/json)

        {
            "questions_url": "/questions"
        }

## Question Resource

Represents a poll question.

### Attributes

*   `question`: The text of the question.
*   `published_at`: An ISO8601 date when the question was published.
*   `url`: The unique URL of the question.
*   `choices`: An array of Choice objects associated with the question.

### Endpoints

*   **GET /questions/{question_id}**: View a specific question's details.
    *   Parameters:
        *   `question_id` (required, number): ID of the Question.
    *   Response 200 (application/json): Details of the question, including choices and vote counts.

## Choice Resource

Represents an answer choice for a poll question.

### Attributes

*   `choice`: The text of the choice.
*   `url`: The unique URL of the choice.
*   `votes`: The number of votes this choice has received.

### Endpoints

*   **POST /questions/{question_id}/choices/{choice_id}**: Vote on a specific choice for a question.
    *   Parameters:
        *   `question_id` (required, number): ID of the Question.
        *   `choice_id` (required, number): ID of the Choice.
    *   Response 201 (application/json): No body content, but a `Location` header pointing to the updated question.

## Questions Collection Resource

Represents a collection of all poll questions.

### Endpoints

*   **GET /questions{?page}**: List all questions.
    *   Parameters:
        *   `page` (optional, number): The page of questions to return.
    *   Response 200 (application/json): An array of Question objects. Includes a `Link` header for pagination.
*   **POST /questions**: Create a new question.
    *   Request Body (application/json):
        *   `question` (string): The question text.
        *   `choices` (array[string]): A collection of choice texts.
    *   Response 201 (application/json): Details of the newly created question, including its URL and initial vote counts (all zero). A `Location` header points to the new question.
