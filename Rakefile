# -*- ruby -*-

def abs(filename)
  File.expand_path filename
end

SVN_DIR = abs "yast-svn"
GIT_LOCAL_DIR = "yast-git"
GIT_PUBLIC_DIR = "/srv/git"

# assumed to be in path
# get it from  http://gitorious.org/svn2git/svn2git
CONVERTOR = "svn-all-fast-export"
MAP = abs "yast.map"
RULES = abs "yast.rules"

def on_nfs?
  system "stat -c %T -f . | grep nfs"
  $?.success?
end

def convert
  if on_nfs?
    puts "Warning, running this on NFS will slow things down"
  end
  min = ""
#  min = "--resume-from 63000"
  sh "#{CONVERTOR} --add-metadata " +
    "--identity-map #{MAP} --rules #{RULES} #{min} " +
    SVN_DIR + " 2>&1 | tee run.log"
end

def timestamped(&block)
  stamp_name = Time.now.strftime "%Y%m%d-%H%M"
  mkdir stamp_name
  chdir stamp_name, &block
end

desc "Run the conversion, in a new directory"
task :default do
  timestamped { convert }
end

desc "Format the README to HTML"
task :doc => "README.html"
file "README.html" => "README.markdown" do |t|
  sh "markdown #{t.prerequisites.first} > #{t.name}"
end
