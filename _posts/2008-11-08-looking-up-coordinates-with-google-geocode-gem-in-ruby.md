---
title: Looking up coordinates with google-geocode gem in Ruby
tags: ruby
layout: post
---

“google-geocode” helps you to look up the coordinates of a given address.

Installing google-geocode

{% highlight bash %}
sudo gem install google-geocode
{% endhighlight %}

Getting your google maps api key: http://code.google.com/apis/maps/signup.html 

Here is the simple code to fetch coordinates of a given address.

{% highlight ruby %}
#!/usr/bin/ruby
require 'rubygems'
require 'google_geocode'
api_key = 'yourapikey'
address = "34357 Istanbul Dolmabahce"
begin
  gc = GoogleGeocode.new(api_key)
  location = gc.locate(address)
  puts "Adress queried: #{address}"
  puts "---------------------------------------"
  puts "Fetched results:"
  puts "latitude: #{location.latitude}"
  puts "longitude: #{location.longitude}"
  puts "Coordinates lat,lon: #{location.coordinates}"
  puts "Adress fetched: #{location.address}"
rescue GoogleGeocode::KeyError => e
  puts "GoogleGeocode::KeyError => #{e.message}"
rescue GoogleGeocode::AddressError => e 
  puts "GoogleGeocode::AddressError => #{e.message}"
rescue GoogleGeocode::Error => e
  puts "GoogleGeocode::Error => #{e.message}"
rescue Exception => e
  puts "Exception => #{$!}"
end
{% endhighlight %}

### Resources:

[google-gecode gem](http://dev.robotcoop.com/Libraries/google-geocode)
