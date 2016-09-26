.
├── Gemfile                             # Bundler's configuration - contains our project's gems
├── Gemfile.lock                        # a snapshot of all of the gems and versions that Bundler installed
├── README.md                           # The project's README file
├── Rakefile                            # Configuration file for Rake command
├── app                                 # Our main application folder
│   ├── assets                          # Where all of your CSS, JavaScript, and image files belong
│   │   ├── config                      # Config folder for the asset pipeline
│   │   │   └── manifest.js             # Configuration file for asset pipeline
│   │   ├── images                      # Where image files go
│   │   ├── javascripts                 # JavaScript folder
│   │   │   ├── application.js          # main JavaScript file that gets loaded from our layout.erb file
│   │   │   ├── cable.js                # (new in Rails 5) Action Cable provides support for WebSockets
│   │   │   └── channels                # (new in Rails 5) Client-side Action Cable Channel code goes here
│   │   └── stylesheets                 # CSS folder
│   │       └── application.css         # Main CSS file
│   ├── channels                        # (new in Rails 5) Action Cable provides support for WebSockets
│   │   └── application_cable           # (new in Rails 5) Server-side Action Cable Channel code goes here
│   │       ├── channel.rb              # (new in Rails 5) Parent class for Channels
│   │       └── connection.rb           # (new in Rails 5) Connection config for Action Cable
│   ├── controllers                     # MVC _Controllers_ go here
│   │   ├── application_controller.rb   # Main Controller class
│   │   └── concerns                    # Where your controller "concerns" go (mixins)
│   ├── helpers                         # Where your helpers go
│   │   └── application_helper.rb       # Main ApplicationHelper class
│   ├── jobs                            # Jobs are for scheduled background processing
│   │   └── application_job.rb          # Main ApplicationJob class
│   ├── mailers                         # EMail support
│   │   └── application_mailer.rb       # Main ApplicationMailer class
│   ├── models                          # MVC _Models_ go here
│   │   ├── application_record.rb       # Main ApplicationRecord class
│   │   └── concerns                    # Where your model "concerns" go (mixins)
│   └── views                           # MVC _Views_ go here
│       └── layouts                     # View layouts go here
│           ├── application.html.erb    # Main Application layout
│           ├── mailer.html.erb         # Mailer HTML layout
│           └── mailer.text.erb         # Mailer text layout
├── bin                                 # Contains configurable commands for developing Rails apps
│   ├── bundle                          # Bundler - installing dependencies
│   ├── rails                           # rails - generating and scaffolding code
│   ├── rake                            # rake - rails make - performs miscellaneous chores
│   ├── setup                           # helps developers quickly "setup" their environment
│   ├── spring                          # Spring is a Rails application preloader
│   └── update                          # Script for updating gems and database schema
├── config                              # Project config directory
│   ├── application.rb                  # Main application config
│   ├── boot.rb                         # Loads all of the gems for the Rails app
│   ├── cable.yml                       # (new in Rails 5) Action Cable config
│   ├── database.yml                    # Database config
│   ├── environment.rb                  # Initialize the Rails application
│   ├── environments                    # Contains environment specific config files
│   │   ├── development.rb              # Config for development
│   │   ├── production.rb               # Config for production
│   │   └── test.rb                     # Config for unit testing
│   ├── initializers                    # Initializes various components of the Rails app
│   │   ├── application_controller_renderer.rb
│   │   ├── assets.rb
│   │   ├── backtrace_silencers.rb
│   │   ├── cookies_serializer.rb
│   │   ├── filter_parameter_logging.rb
│   │   ├── inflections.rb
│   │   ├── mime_types.rb
│   │   ├── new_framework_defaults.rb
│   │   ├── session_store.rb
│   │   └── wrap_parameters.rb
│   ├── locales                          # Contains files for internationalization
│   │   └── en.yml
│   ├── puma.rb                          # Config for the puma server
│   ├── routes.rb                        # RESTful route configuration
│   ├── secrets.yml                      # Keys used for signing cookies
│   └── spring.rb                        # spring config
├── config.ru                            # used by Rack-based servers
├── db                                   # Contains files for seeding the DB
│   └── seeds.rb                         # Default seeds file
├── lib
│   ├── assets
│   └── tasks
├── log                                  # Contains log files - used for debugging
│   └── development.log
├── public                               # Static client-side files not processed by the Asset Pipeline
│   ├── 404.html
│   ├── 422.html
│   ├── 500.html
│   ├── apple-touch-icon-precomposed.png
│   ├── apple-touch-icon.png
│   ├── favicon.ico
│   └── robots.txt
├── tmp                                  # tmp folder contains files that get generated
│   ├── cache
│   │   └── assets
│   ├── pids
│   ├── restart.txt
│   └── sockets
└── vendor                               # Contains 3rd party files
    └── assets
        ├── javascripts
        └── stylesheets
