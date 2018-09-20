# Practica9_Semaforo
#define sRojo 12
#define sAmarillo 11
#define sVerde 10
#define pRojo 9
#define pVerde 8
#define boton 7
#define potenciometro A0
boolean estadoBoton = false;  // Variable que guarda el estado del pushBotton cuando es o no oprimido
int valorPot = 0; // Variable que almacena el valor en el que se encuentra el potenciometro
int tiempoEsperaPot = 10; // Variable que se multiplica con el valor del potenciometro para asignar su tiempo de espera antes del cambio de estado del semaforo
int tiempo = 0; // Variable que se utiliza como tiempo de espera antes de los cambios de estado del semaforo

void setup() {
  Serial.begin(9600);
  pinMode(sRojo, OUTPUT);
  pinMode(sAmarillo, OUTPUT);
  pinMode(sVerde, OUTPUT);
  pinMode(pRojo, OUTPUT);
  pinMode(pVerde, OUTPUT);
  pinMode(boton, INPUT);
}

void loop() {
  Serial.println(valorPot);
  valorPot = analogRead(potenciometro);
  digitalWrite (pRojo, HIGH);
  digitalWrite (sVerde, HIGH);
   
  estadoBoton = digitalRead(boton);
  if (estadoBoton == true) {
    esperaPotenciometro();
    secuenciaSemaforo();
  }
}

void secuenciaSemaforo() {
  for (int i = 0; i < 3; i++) {
    digitalWrite(sVerde, LOW);
    delay(1000);
    digitalWrite(sVerde, HIGH);
    delay(1000);
    digitalWrite(sVerde, LOW);
  }
  digitalWrite(sAmarillo, HIGH);
  delay(2500);
  digitalWrite(sAmarillo, LOW);
  digitalWrite(sRojo, HIGH);
  delay(2000);
  digitalWrite(pRojo, LOW);
  digitalWrite(pVerde, HIGH);
  delay(tiempo);
  secuenciaPeaton();
  digitalWrite(pRojo, HIGH);
  delay(2000);
  digitalWrite(sRojo, LOW);
  digitalWrite(pVerde, LOW);
}
 
void secuenciaPeaton() {
  for (int i = 0; i < 3; i++) {
    digitalWrite(pVerde, LOW);
    delay(1000);
    digitalWrite(pVerde, HIGH);
    delay(1000);
    digitalWrite(pVerde, LOW);
    
  }
}

void esperaPotenciometro() {
  valorPot = valorPot * tiempoEsperaPot; 
  Serial.println(valorPot);
  digitalWrite(sVerde, HIGH);
  tiempo = map(valorPot, 0, 1023, 10, 25)*200;
  Serial.println(tiempo);
  delay(tiempo);
}
