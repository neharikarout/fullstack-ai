![alt text](image-2.png)

## pydantic - to define schema

    @app.post("/createposts")
    def create_posts(payLoad: dict = Body(...)):
        print(payLoad)
        return {"new_post": f"title: {payLoad['title']} content: {payLoad['content']}"}

 -- what data we expect from user - title str, content str


 so we do following:
-- from pydantic import BaseModel
-- class Post(BaseModel): # defined schema
        title: str
        content: str
 -- @app.post("/createposts")
        def create_posts(new_post: Post): 
            print(new_post)  # title='top beaches in florida' content='checkout the beaches'
            print(new_post.title) # top beaches in florida
            return {"data": "new post"}

    # here put the check 'Post' and stored it in new post



## inbuilt validation check in fastAPI through pydantic
-- fastAPI sends the validation error on its own 
-- status code : 422- unprocessable entity
-- INFO: 127.0.0.1:55237 - "POST /createposts HTTP/1.1" 422 Unprocessable Entity
    {
        "detail": [
            {
                "type": "missing",
                "loc": [
                    "body",
                    "title"
                ],
                "msg": "Field required",
                "input": {
                    "content": "checkout the beaches"
                }
            }
        ]
    }


## for optional fields

    class Post(BaseModel):
        title: str
        content: str
        published: bool = True  # here we have provided a default value
        rating: Optional[int] = None # fully optional (import Optional from typing)


## to convert the extracted data from pydantic model to json/dict
-- use dict() - deprecated now model_dump() is being used
   
    @app.post("/createposts")
    def create_posts(new_post: Post):
        print(new_post.rating)
        print(new_post.model_dump())
        return {"data": "new post"}


