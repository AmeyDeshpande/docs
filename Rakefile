# Directories and files generated by this Rakefile
#   - _includes: Jekyll includes
#   - assets/images/docs: Images for the docs repo
$generated_files = [
    '_data',
    '_includes',
    '_layouts',
    '_plugins',
    'assets/',
    'assets/images/docs',
]

# ---- Rake tasks
task :default => :serve

desc 'Set up the build environment'
task :init do
    # Install packages
    sh 'bundle install'
end

# Copy assets needed for the Jekyll build
desc 'Copy assets and includes for the Jekyll build'
task :copy_assets do
    # Create each destination directory, if it doesn't already exist
    ['_data/docs','_includes','assets/images/docs'].each{ |dir_name|
        FileUtils.mkdir_p(dir_name) unless Dir.exists?(dir_name)
    }

    assets_to_copy = [
        {:source => '_jekyll/_standalone/assets/.', :target => 'assets/'},
        {:source => '_jekyll/_images/.', :target => 'assets/images/docs/'},
        {:source => '_jekyll/_data/.', :target => '_data/docs/'},
        {:source => '_jekyll/_includes/.', :target => '_includes/docs/'},
        {:source => '_jekyll/_standalone/_layouts/.', :target => '_layouts/'},
        {:source => '_jekyll/_standalone/_plugins/.', :target => '_plugins/'},
    ]
    assets_to_copy.each{ |asset|
        FileUtils.cp_r(asset[:source], asset[:target], :verbose => true)
    }
end

desc 'Clean up the generated site'
task :clean do
    rm_rf '_site'
    rm '.jekyll-metadata', :force => true
    $generated_files.each{ |d|
        rm_rf d
    }
end

desc 'Build site with Jekyll'
task :build => ['clean', 'copy_assets'] do
    jekyll('build')
end

desc 'Start server and regenerate files on change'
task :serve => ['copy_assets'] do
    check_for_required_files(:warning => true)
    jekyll('serve') 
end

# ---- Rake functions

# Run Jekyll
def jekyll(opts = '')
    if ENV['dev']=='on'
        dev = ' --plugins=_plugins,_plugins-dev'
    else
        dev = ''
    end
    sh "bundle exec jekyll #{opts}#{dev} --trace"
end

# Check if all generated files are present: by default abort if files aren't present, otherwise show a warning
def check_for_required_files(opts={})
    missing_files = 0
    $generated_files.each do |f|
        if !File.exists?(f)
            puts "Required file missing: #{f}"
            missing_files +=1
        end
    end
    if missing_files > 0
        error = "#{missing_files} required files not found. Run `rake build` before deploying."
        if opts[:warning] then puts error else fail error end
    end
end
