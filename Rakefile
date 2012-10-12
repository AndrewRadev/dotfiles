def file_mapping(root = getwd)
  root = File.expand_path(root)

  [
    ['~/.Xdefaults',                'home/Xdefaults'],
    ['~/.Xmodmap',                  'home/Xmodmap'],
    ['~/.xinitrc',                  'home/xinitrc'],
    ['~/archive/Xdefaults',         'archive/Xdefaults'],
    ['~/.ncmpcpp/config',           'home/ncmpcpp_config'],
    ['~/.pentadactylrc',            'home/pentadactylrc'],
    ['~/.vimperatorrc',             'home/vimperatorrc'],
    ['~/.ctags',                    'home/ctags'],
    ['~/.ackrc',                    'home/ackrc'],
    ['~/.vimproj',                  'home/vimproj'],
    ['~/.config/user-dirs.dirs',    'home/config_user_dirs.dirs'],
    ['~/.rvm/gemsets/default.gems', 'home/rvm_default_gems'],
    ['~/bin/backup',                'bin/backup'],
    ['~/bin/extract-audio',         'bin/extract-audio'],
    ['~/bin/firefox-optimize',      'bin/firefox-optimize'],
    ['~/bin/install-rvm',           'bin/install-rvm'],
    ['~/bin/random-matrices',       'bin/random-matrices'],
    ['~/bin/random-password',       'bin/random-password'],
    ['~/bin/sound',                 'bin/sound'],
    ['~/bin/translate',             'bin/translate'],
    ['~/bin/whenis',                'bin/whenis'],
    ['/etc/X11/xorg.conf.d/',       'etc/xorg.conf.d/'],
    ['/etc/mpd.conf',               'etc/mpd.conf'],
  ].map do |pair|
    pair.map { |file| File.expand_path(file, root) }
  end
end

def timestamp
  Time.now.strftime("%Y_%m_%d_%H%M%S")
end

def copy_files(source, destination)
  prepare_dir(File.dirname(destination))
  system "sudo cp -av #{source} #{destination}"
end

def prepare_dir(dir)
  dir = File.expand_path(dir)
  if not File.exists?(dir)
    puts "> Creating #{dir}"
    mkdir_p dir
  end
  dir
end

desc "Show all stored files"
task :list do
  FileList['*/**'].each { |file| puts file }
end

desc "Create a backup in ~/dotfiles_backup with all files currently in place"
task :backup do
  backup_dir = prepare_dir "~/dotfiles_backup/#{timestamp}"
  file_mapping(backup_dir).each do |local, stored|
    copy_files local, stored if File.exists?(local)
  end
end

desc "Take the files from the machine and copy them to storage"
task :take do
  file_mapping('./storage').each do |local, stored|
    if File.exists? local
      rm_rf stored if File.exists? stored
      copy_files local, stored
    else
      warn "> Couldn't find #{local}"
    end
  end
end

desc "Take the files from storage and copy them locally"
task :put => :backup do
  file_mapping('./storage').each do |local, stored|
    if File.exists? stored
      rm_rf local if File.exists? local
      copy_files stored, local
    else
      warn "> Couldn't find #{stored}"
    end
  end
end
