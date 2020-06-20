# Answer
### Review and give your through about it.
```ruby
  # please improve this rails action,
  # if you'd like to put some code in another file please state so:
  # authentication API
  def auth
      user = User.find(params[:username])
    if user.check_password(params[:password])
      render json: user
    else
      render json: { errors: ["wrong username or password"] }, status: :unauthorized
    end
  end
```

1. Syntax Error
```ruby
  user = User.find(params[:username])
```
This code will cause an error, because the `.find` method will display 1 record from the model
`User` or the` users` table by using `id` as a condition of an integer type, while what we enter is a string with the` username parameter` then this code will cause an error when we look for an id but we look for a username.
We should be able to use:

```ruby
  # if you want to use ID
  user = User.find(params[:id])
  
  # if you want to use a username
  user = User.find_by(username: params[:username])
```

2. Status Code
```ruby
  render json: user
```
Add parameters to return the status of the code, for example if an error does not occur we can reset
with the status code `:ok`, it's better to use`:ok` than `200`.

3. Standarisasi Response
```ruby
  if user.check_password(params[:pasword])
    render json: user
  else
    render json: { errors: ['Wrong username or password'] }, status: :unauthorized
  end
```
Make a format response so that the response is the same as when an error occurs or there is no error.
Examples such as:
```ruby
  render json: { status: 'OK', results: nil, errors: ['Wrong username or password.'] }`  
```
and if the format is the same, we can format it in 1 method so that our code can DRY, for example like:
```ruby
  # method for format response
  def render_response(data, status_code)
    render json: { status: data[0], results: data[1], errors: data[2] }, status: status_code
  end

  # example use
  response = ['FAIL', nil, ['Wrong username or password.']]
  render_response(response, :unauthorized)
```

### Please explain how rails routing works, how is it different from routing in Grape API? And how they compare?

1. Rails  
Rails uses routing mechanisms like other frameworks,
for example we define route with the format: 
```
  http method | URI | action method 
```
Routing functions are more general not for making certain applications, such as REST API applications or not REST APIs, so they are more global.

2. Grape  
Grape has a more specific mechanism, grape is intended for making applications based on REST API,
making routing on grape is better and suitable if we want to create applications based on REST API,
like for example if we want to make an API versioning.

3. Compare  
So the conclusion is that each framework has its own advantages, grape is more suitable if we want to create REST API based applications
because grape has features that make it easy for us to create REST API based applications.
because routing for REST API applications requires more attention.  


