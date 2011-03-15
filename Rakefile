# -*- ruby -*-

def abs(filename)
  File.expand_path filename
end

SVN_DIR = "yast-svn"
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
  min = "--resume-from 60000"
  sh "#{CONVERTOR} --add-metadata " +
    "--identity-map #{MAP} --rules #{RULES} #{min} " +
    SVN_DIR + " 2>&1 | tee run.log"
end

def timestamped(&block)
  stamp_name = Time.now.strftime "%Y%m%d-%H%M"
  mkdir stamp_name
  chdir stamp_name, &block
end

task :default do
  timestamped { convert }
end
