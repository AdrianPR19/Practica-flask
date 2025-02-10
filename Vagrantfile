Vagrant.configure("2") do |config|
  
  config.vm.define "flask" do |flask|
    flask.vm.box = "debian/bullseye64"
    flask.vm.hostname = "flask"  
    flask.vm.network "private_network", ip: "192.168.56.10"  
    
    flask.vm.provision "shell", privileged: true, inline: <<-SHELL
      sudo apt-get update && sudo apt-get install -y python3-pip
    SHELL
    
    flask.vm.provision "shell", privileged: false, inline: <<-SHELL
      pip3 install --user pipenv
      pip3 install --user python-dotenv
    SHELL
    
    flask.vm.provision "shell", privileged: true, inline: <<-SHELL
      sudo mkdir -p /var/www/app
      sudo chown -R vagrant:www-data /var/www/app
      sudo chmod -R 775 /var/www/app
    SHELL
    
    flask.vm.provision "shell", privileged: false, inline: <<-SHELL
      echo "FLASK_APP=wsgi.py" > /var/www/app/.env
      echo "FLASK_ENV=production" >> /var/www/app/.env
      cd /var/www/app && pipenv shell && pipenv install flask gunicorn
      touch /var/www/app/application.py /var/www/app/wsgi.py
      echo "from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    '''Index page route'''
    return '<h1>App desplegada</h1>'" > /var/www/app/application.py
      
      echo "from application import app

if __name__ == '__main__':
   app.run(debug=False)" > /var/www/app/wsgi.py
    SHELL
  end
end
