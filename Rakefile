require 'csv'
require 'date'

@error_file = 'www-clir-org_20171219T140157Z_CrawlErrors.csv'
@redirect_file = '.htaccess'
@reports_file = 'reports.csv'

def timestamp(date = Time.now)
  date.strftime('%Y-%m-%d-%H-%M-%S')
end


def write_redirects(contents, file)
  File.open(file, 'w') { |file|
    contents.each do |line|
      file.write(line + "\n")
    end
  }
end

def clean_redirects
  table = CSV.table(@redirect_file)

  table.delete_if do |row|
    row['Response Code'] != 404
  end

  File.open(@redirect_file, 'w') do |f|
    f.write(table.to_csv)
  end
end

desc 'Massage Report URLs'
task :reports do
  CSV.foreach(@reports_file, headers: true) do |row|

    url = row['URL']

    if url.end_with?('.pdf')
      puts "/wp-content/uploads/sites/6/#{File.basename(url)}"
    end

  end
end

desc 'Generate Redirects'
task :redirects do

  redirects = []

  CSV.foreach(@error_file, headers: true) do |row|
    structure = {
      url: row['URL'],
      redirect: row['Redirect'],
      response: row['Response Code']
    }

    redirects << "Redirect #{structure[:response]} #{structure[:url]} #{structure[:redirect]}" unless structure[:response] == '404'

  end

  write_redirects(redirects, ".htaccess-#{timestamp}")
  clean_redirects
end
