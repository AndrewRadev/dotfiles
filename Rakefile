def file_mapping
  [
    ['~/.Xdefaults',             'home/Xdefaults'],
    ['~/.Xmodmap',               'home/Xmodmap'],
    ['~/.xinitrc',               'home/xinitrc'],
    ['~/archive/Xdefaults',      'archive/Xdefaults'],
    ['~/.ncmpcpp/config',        'home/ncmpcpp_config'],
    ['~/.pentadactylrc',         'home/pentadactylrc'],
    ['~/.vimperatorrc',          'home/vimperatorrc'],
    ['~/.ctags',                 'home/ctags'],
    ['~/.config/user-dirs.dirs', 'home/config_user_dirs.dirs'],
    ['~/bin/backup',             'bin/backup'],
    ['~/bin/extract-audio',      'bin/extract-audio'],
    ['~/bin/firefox-optimize',   'bin/firefox-optimize'],
    ['~/bin/install-rvm',        'bin/install-rvm'],
    ['~/bin/random-matrices',    'bin/random-matrices'],
    ['~/bin/random-password',    'bin/random-password'],
    ['~/bin/sound',              'bin/sound'],
    ['~/bin/translate',          'bin/translate'],
    ['~/bin/whenis',             'bin/whenis'],
    ['/etc/X11/xorg.conf.d',     'etc/xorg.conf.d'],
    ['/etc/mpd.conf',            'etc/mpd.conf'],
  ].map do |pair|
    pair.map { |file| File.expand_path(file) }
  end
end

def local_files
  file_mapping.map { |f| f[0] }
end

def stored_files
  file_mapping.map { |f| f[1] }
end

def timestamp
  Time.now.strftime("%Y_%m_%d_%H%M%S")
end

def prepare_dir(dir)
  dir = File.expand_path(dir)
  if not File.exists?(dir)
    puts "> Creating #{dir}"
    mkdir_p dir
  end
  dir
end

def basename(file)
  File.basename(file)
end

task :list do
  FileList['*/**'].each do |file|
    puts file
  end
end

task :backup do
  backup_dir = prepare_dir "~/dotfiles_backup/#{timestamp}"
  local_files.each do |file|
    cp_r file, File.join(backup_dir, basename(file)) if File.exists?(file)
  end
end

task :take do
  file_mapping.each do |local, stored|
    if File.exists? local
      rm_rf stored if File.exists? stored
      cp_r local, stored
    else
      warn "> Couldn't find #{local}"
    end
  end
end

task :put => :backup do
  file_mapping.each do |local, stored|
    if File.exists? stored
      rm_rf local if File.exists? local
      cp_r stored, local
    else
      warn "> Couldn't find #{stored}"
    end
  end
end
