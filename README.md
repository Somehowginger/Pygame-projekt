Pygame-projekt
Julian og Emil 2.P

Python (import pygame)
Visuel Studio Code

Spil antal tegn = 7319
Read me antal tegn = 

Vi har lavet et simpelt run and jump spil. Man spiller som dreng som skal hoppe over snegle og undvige fluer i luften. Hvis man bliver ramt af sneglen eller fluen slutter og spillet og man kan se sin score og spille igen.
Vi har et player inputs i vors spil, space/mellemrum. Space har to handlinger i vores spil. Når man spiller selve spillet, bliver space brugt til at hoppe. Hvis man er på startscreen/game over screen, så bruger man space til at starte spillet igen.
Der er en score i spillet. Den bliver både vist når man spiller spilet men også når man er på game over screen.


Vi har rect alle vores sprites og billeder. Det er nemmere at placere rect end det øverste venstre hjørne som det normalt er. Det gør det også nemmere at lave kollision. 
def collisions(player,obstacles):
	if obstacles:
		for obstacle_rect in obstacles:
			if player.colliderect(obstacle_rect): return False
	return True
Koden tjekker om firekanterne er inde i hinanden. Hvis de er bruger vi randint fra random som giver os return. 
Den overstående kode står ikke inde i while True:  men randint gør så vi ikke behøver at skrive så meget i while True:
 

Vi har også importet exit fra sys. Det giver os kommandoen. 
if event.type == pygame.QUIT: 
  pygame.quit()   
  exit()
Det gør det mere simpelt at lukke spillet


Choice fra random har vi også importet. Det giver os mulighed for vores obstacles at komme tilfældigt nemt. 
if game_active:
			if event.type == obstacle_timer:
				obstacle_group.add(Obstacle(choice(['fly','snail','snail','snail'])))
Her bestemmer programmet selv hvilken obstacle der skal komme som den næste. Vi har valgt, at der kun skal være 25 procent chance, for en flue siden man ikke skal reagere på den.


Vi ville gerne lave en startsscreen/game over screen, så vi lavede variablen game_active
Når man starter spillet er game_active=False. Det vil sige vi starter på startscreen. Hvis man så klikker space bliver game_active=True. Så starter spillet.

if score == 0: screen.blit(game_message,game_message_rect)
else: screen.blit(score_message,score_message_rect)
De 2 linjer kode gør hvis man har en score på 0 står der "Press space to run". Siden man starter spillet med en score på 0 står der altid "Press space to run" første gang. 
Scoren når også at blive 1 før man skal dø til den, hvilket gør så man kun får "Press space to run" en gang, og det er den første gang.
Ellers står der ens score.





