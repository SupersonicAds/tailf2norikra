$LOAD_PATH.unshift File.expand_path("../lib", __FILE__)
require "tailf2norikra/version"
 
task :build do
  system "gem build tailf2norikra.gemspec"
end
 
task :release => :build do
  system "gem push tailf2norikra-#{Tailf2Norikra::VERSION}.gem"
end
