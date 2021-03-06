= CalendarHelper

== DESCRIPTION:

A simple helper for creating an HTML calendar. The "calendar" method will be automatically available to your Rails view templates, or can be used with Sinatra or other webapps.

There is also a Rails generator that copies some stylesheets for use alone or alongside existing stylesheets.

== SYNOPSIS:

  # Simple
  calendar(:year => 2005, :month => 6)
  
  # Set table class
  calendar({:year => 2005, :month => 6, :table_class => "calendar_helper"})
  
  # Full featured
  calendar(:year => 2005, :month => 5) do |d| # This generates a simple calendar, but gives special days
    if listOfSpecialDays.include?(d)          # (days that are in the array listOfSpecialDays) one CSS class,
      [d.mday, {:class => "specialDay"}]      # "specialDay", and gives the rest of the days another CSS class,
    else                                      # "normalDay". You can also use this highlight today differently
      [d.mday, {:class => "normalDay"}]       # from the rest of the days, etc.
    end
  end

  With texts and links in special days

  @listOfSpecialDays = [ '2010-11-25'.to_date, '2010-11-29'.to_date ]
  @listOfSpecialDetails = {
    '2010-11-25' => [:day => '2010-11-25', :label => "<br />A text with <a href='#'>link</a>."],
    '2010-11-29' => [:day => '2010-11-29']
  }

  calendar({:year => 2010, :month => 11}) do |d|
    if @listOfSpecialDays.include?(d)
      [d.mday, {:class => "specialDay", :label => @listOfSpecialDetails[d.to_s][0][:label] } ]
    else
      [d.mday, {:class => "normalDay", :label => ''} ]
    end
  end

If using with ERb (Rails), put in a printing tag.

  <%= calendar(:year => @year, :month => @month, :first_day_of_week => 1) do |d|
        render_calendar_cell(d)
      end
  %>

With Haml, use a helper to set options for each cell.

  = calendar(:year => @year, :month => @month, :first_day_of_week => 1) do |d|
    - render_calendar_cell(d)

In Sinatra, include the CalendarHelper module in your helpers:

  helpers do
    include CalendarHelper
  end

== AUTHORS:

Jeremy Voorhis -- http://jvoorhis.com
Original implementation

Geoffrey Grosenbach -- http://nubyonrails.com
Test suite and conversion to a Rails plugin

== Contributors:

* Jarkko Laine http://jlaine.net/
* Tom Armitage http://infovore.org
* Bryan Larsen http://larsen.st

== USAGE:

See the RDoc (or use "rake rdoc").

To copy the CSS files, use

  ./script/generate calendar_styles

CSS will be copied to subdirectories of public/stylesheets/calendar.

