Når vi skal kalkulere arealet under grafen, så har du sett i geogebra-appleten i forrige delkapittel at vi kan bruke rektangler eller trapeser. Dette er metodene vi i hovedsak skal bruke. I tillegg finnes det en metode til som heter Simpsons metode som er den foretrukne metoden for de fleste tilfeller. Under ser du en geogebra-applet som du kan bruke til å utforske de forskjellige metodene vi går igjennom her.

<iframe scrolling="no" title="Numeriske metoder" src="https://www.geogebra.org/material/iframe/id/rbn9947z/width/650/height/461/border/888888/sfsb/true/smb/false/stb/false/stbh/false/ai/false/asb/false/sri/false/rc/false/ld/false/sdz/false/ctl/false" width="650px" height="461px" style="border:0px;"> </iframe>

Vi skal nå i tur og orden definere og begrunne de forskjellige metodene både matematisk og med kode. Hvilken av metodene som er best, skal vi undersøke i oppgavene.

=== Rektangelmetoden ===
Rektangelmetoden er den samme metoden som vi brukte for å definere bestemte integraler i forrige seksjon. Vi så også på hvordan vi kunne regner ut arealet til 4 søyler. La oss nå gi en mer generell definisjon.
!bnotice Matematisk formel for rektangelmetoden.
\[
\int_{a}^{b}f(x)\,dx \approx \sum_{n=1}^{N}f(x_n)\cdot h
\]
hvor $a$ er $x$-verdien vi starter på, $b$ er $x$-verdien vi avslutter på, $N$ er antall rektangler og $h$ er bredden til hvert rektangel (det vi har kalt $\Delta x$ før).
!enotice
Vi kan implementere koden som en python-funksjon som tar imot den matematiske funksjonen, $a$, $b$ og hvor mange rektangler vi ønsker. Det er kun her jeg har med mange kommentarer ettersom alle påfølgende koder har samme strukturen. I koden under regner vi ut $\int_{0}^{3}x^2\,dx$
!bc pypro
def rektangel(f,a,b,N):     #Funksjon for rektangelmetoden
    areal = 0               #Startverdi for areal
    x = a                   #Startverdi for x
    h = (b-a)/N             #Kalkulering av steglengde
    for i in range(1,N+1):  #Løper over alle verdier av x_n
        areal += f(x)*h     #Legger til arealet der vi er til det gamle.
        x += h              #Oppdaterer x med en steglengde
    return areal            #Returnerer det totale arealet.

def f(x):                   #Funksjonen vi skal finne arealet til.
    return x**2

xstart = 0                  #Nedre grense for integral
xslutt = 3                  #Øvre grense for integral
Nrekt = 1000                #Hvor mange rektangler vi skal ha

ans = rektangel(f,xstart,xslutt,Nrekt)    #Svaret etterfulgt av en fin print.
print("Integralet av x^2 for x-verdier mellom %g og %g er %g"
      %(xstart,xslutt, ans))
!ec
=== Trapesmetoden ===
I introduksjonen i forrige delkapittel så du muligheten til å bruke både rektangler og trapeser til å finne arealet under en graf (hvis du ikke husker dette, scroll opp til geogebra-appleten). Vi starter med å se på den matematiske formelen.
!bnotice Matematisk definisjon for trapesmetoden.
\[
\int_{a}^{b}f(x)\,dx \approx \sum_{n=1}^{N}\frac{f(x_{n})+f(x_{n+1})}{2}\cdot h
\]
Der $a$ og $b$ er som tidligere, og $\frac{f(x_{n})+f(x_{n+1})}{2}\cdot h$ er arealet av trapes nummer $n$ og $N$ er det totale antallet trapeser.
!enotice
La oss lage en python-funksjon av denne metoden og bruke det til å regne ut $\int_{0}^{3.14}\sin x\,dx$.
!bc pypro
from math import *
def Trapesmetoden(f,a,b,N):
  areal = 0
  x = a
  h = (b-a)/N
  for i in range(1,N+1):
    areal += (f(x) + f(x+h))/2 * h
    x += h
  return areal

def f(x):
  return sin(x)

xstart = 0
xslutt = 3.14
Ntrap = 1000

ans = Trapesmetoden(f,xstart,xslutt,Ntrap)
print("Integralet av sin(x) for x-verdier mellom %g og %g er %g"
      %(xstart,xslutt, ans))
!ec

=== Midtpunktsmetoden ===
Midtpunktsmetoden baserer seg på rektangler, men istedet for å la høyden til rektanglene være bestemt av $f(x_{n})$, så velger vi heller funksjonsverdien midt mellom to $x$-verdier. Vi får derfor:
!bnotice Midtpunktsmetoden
\[
\int_{a}^{b}f(x)\,dx \approx \sum_{n=1}^{N}f(x_n + 0.5h)\cdot h
\]
!enotice
La oss anvende denne på funksjonen $f(x)=\sqrt{1+x^{2}}$
!bc pypro
from math import *
def Midtpunkt(f,a,b,N):
  areal = 0
  x = a
  h = (b-a)/N
  for i in range(1,N+1):
    areal += f(x+0.5*h)*h
    x += h
  return areal

def f(x):
  return sqrt(1+x**2)

xstart = 0
xslutt = 10
Nrekt = 1000

ans = Midtpunkt(f,xstart,xslutt,Nrekt)
print("Integralet av sqrt(1+x**2) for x-verdier mellom %g og %g er %g"
      %(xstart,xslutt, ans))
!ec

=== Simpsons metode ===
Simpsons metode er en litt mer avansert algoritme som ofte er den matematiske programmer benytter seg av når de skal integrere. Den kommer i mange varianter, men vi fokuserer på den letteste. Formelen for Simpons metode tar utgangspunkt i noe vi kaller for et "vektet gjennomsnitt" av midtpunktsmetoden og trapesmetoden. Vi kan skrive dette som en regel:
!bnotice Enkel formulering av Simpson's metode
\[\int_{a}^{b}f(x)\,dx \approx \frac{2M+T}{3} = \sum_{n=1}^{N}\frac{h}{6}\left(f(x_{n})+4f(x_{n} + 0.5h)+f(x_{n+1})\right)\]
Hvor $M$ er midtpunktsmetoden og $T$ er trapesmetoden.
!enotice
La oss forsøke å regne ut $\int_{2}^{5}(x^{3}+3x^{2})\,dx$ i python. Vi gjør dette på to måter.
<linebreak>
__ Alternativ 1:__ Vi bruker trapesmetoden og midtpunktsmetoden separat.
!bc pypro
def M(f,a,b,N):           #Funksjon for midtpunktsmetoden
  areal = 0
  x = a
  h = (b-a)/N
  for i in range(1,N+1):
    areal += f(x+0.5*h)*h
    x += h
  return areal

def T(f,a,b,N):         #Funksjon for trapesmetoden.
    areal = 0
    x = a
    h = (b-a)/N
    for i in range(1,N+1):
      areal += (f(x) + f(x+h))/2 * h
      x += h
    return areal

def Simpson(f,a,b,N):
  return ( 2*M(f,a,b,N) + T(f,a,b,N) )/3

def f(x):
  return x**3 + 3*x**2

xstart = 0
xslutt = 10
N = 1000

ans = Simpson(f,xstart,xslutt,N)
print("Integralet av x**3 + 3*x**2 for x-verdier mellom %g og %g er %g"
      %(xstart,xslutt, ans))
!ec
__ Alternativ 2: __ Direkte med formel.
!bc pypro
def Simpson(f,a,b,N):
  areal = 0
  x = a
  h = (b-a)/N
  for i in range(1,N+1):
    areal += h/6*(f(x) + 4*f(x + 0.5*h) + f(x+h))
    x += h
  return areal

def f(x):
  return x**3 + 3*x**2

xstart = 2
xslutt = 5
N = 1000

ans = Simpson(f,xstart,xslutt,N)
print("Integralet av x**3 + 3*x**2 for x-verdier mellom %g og %g er %g"
      %(xstart,xslutt, ans))
!ec