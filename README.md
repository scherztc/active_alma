# ActiveSierra

A Rails engine with models for the ExLibris Inc. Alma and Primo  integrated library system with API. 

## Installation

Add this line to your application's Gemfile:

    gem 'active_alma'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install active_alma

Additionally, add 'alma' to your config/database.yml, to connect to your local database:

    alma:
        adapter: postgresql
        database: iii
        host: your_database_url
        port: your_database_port
        schema_search_path: alma_view
        pool: 5
        timeout: 5000
        username: your_username
        password: your_password
        sslmode: require

## Usage

The engine provides models for major records types in the Alma database, per the [Alma documentation](https://developers.exlibrisgroup.com/alma/apis/) (authentication required).

Record models currently include:
- BibView
- ItemView
- OrderView
- VarfieldViews
- Subfields

BibRecord, ItemRecord, and OrderRecord are also included because they respond more quickly to certain queries

Relationships between records are included, e.g.:

    b = BibView.first
    i = b.item_views
    b = i.bib_views ## Returns all attached bib records (items can have more than one attached bib record)
    o = OrderView.first
    b = o.bib_view ## Returns single attached bib record (orders, and checkins can only have one attached bib)

All records have attached variable fields, expressed through the VarfieldViews model:

    v = b.varfield_views

And variable fields each have sub-fields:

    v.subfields

The \*View/\*Record types also have creation\deletion data in an associated table:

    b.record_metadata
