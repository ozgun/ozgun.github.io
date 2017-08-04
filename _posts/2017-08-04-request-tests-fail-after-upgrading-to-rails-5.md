---
title: Request tests fail after upgrading to Rails 5
tags: rails
layout: post
---

Before Rails 5, I used to write "request specs" using rspec as below:

{% highlight ruby %}

describe Api::Apps::Show do

  describe 'GET /api/apps/:id' do

    it 'responds with 200' do

      headers = { 
        'Authorization' => "Bearer #{auth_token}",  
        "Content-Type" => "application/vnd.api+json",
        "Accept" => "application/json"
      }

      get url, nil, headers
      expect(response.status).to eq(200)
    end

  end

end

{% endhighlight %}

After upgrading to Rails 5, when I run the spec above I got the following error:

{% highlight text %}
ArgumentError: wrong number of arguments (given 3, expected 1)
{% endhighlight %}

According to the error message above, `get` method expects 1 argument instead of 3. Trying to debug the error was redirected me to `actionpack-5.1.3/lib/action_dispatch/testing/integration.rb` file. I found the definition of `get` method around line 15:

{% highlight ruby %}
def get(path, **args)
  process(:get, path, **args)
end
{% endhighlight %}

Replacing the `get` method call in the spec code as below solved the problem:

{% highlight ruby %}
get url, params: nil, headers: headers
{% endhighlight %}

See this [stackoverflow post](https://stackoverflow.com/questions/18289152/what-does-a-double-splat-operator-do) how to use splat and double splat.
