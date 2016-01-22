# Tailf2Norikra

Watch and tail files in dirs with specified filename time based patterns and send them to norikra.


## Installation

Add this line to your application's Gemfile:

    gem 'tailf2norikra'

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install tailf2norikra

## Usage

    $ tailf2norikra -h
    Usage: tailf2norikra [options]
            --config PATH                Path to settings config
        -h, --help                       Display this screen
    $

## Config

    tailf:
      files:
        - target: delivery
          prefix: /var/log/app/events
          suffix: ''
          time_pattern: ".%Y-%m-%d"
          max_batch_lines: 48
          timestamp_field: time
          prune_events_older_than: 10
      position_file: "/var/lib/app/tail2norikra.offsets"
      flush_interval: 1
      max_batch_lines: 1024
      from_begining: false
      delete_old_tailed_files: true
    norikra:
      host: localhost
      port: 6666
      send: true

* norikra.host - Norkra server
* norikra.port - Norikra port
* tailf.position_file - file where to save tailed files offsets which were sent to norikra
* tailf.flush_interval - how often in seconds to save the offsets to a file
* tailf.max_batch_lines - max number of lines to batch in each send request
* tailf.from_beggining - in case of a new file added to tailing , if to start tailing from beggining or end of the file
* tailf.delete_old_tailed_files - if to delete files once their time_pattern does not match the current time window and if they have been fully sent to norikra
* tailf.files - array of file configs for tail, each tailed file configs consists of:
  * target - which target to send the events to
  * prefix - the files prefix to watch for
  * time_pattern - ruby time pattern of files to tail
  * suffix - optional suffix of files to watch for
so the tool will watch for files that match - prefix + time_pattern + suffix
  * max_batch_lines - max number of lines to batch in each send request just for this specific file pattern
  * timestamp field: field which comatains timestamp/date
  * prune_events_older_than: events with timestamp older than specified number of seconds will be ignored


## Features/Facts

* The config is validated by [schash](https://github.com/ryotarai/schash) gem
* Tailed files are watched for changes by [rb-notify](https://github.com/nex3/rb-inotify) gem
* Dirnames of all files prefixes are watched for new files creation or files moved to the dir and are automaticaly
added to tailing.
* As well dirnames are watched for deletion or files being moved out of directory, and they are removed from the  list of files watched for changing.
* Based time_pattern, files are periodicaly autodeleted , thus avoiding need for log rotation tools.
* Files are matched by converting time_pattern to a regexp

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
6. Go to 1
