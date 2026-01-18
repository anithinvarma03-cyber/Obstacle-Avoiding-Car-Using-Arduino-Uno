# Obstacle Avoiding Car Using Arduino Uno

## 📌 Project Overview
This project focuses on building an autonomous obstacle avoiding car using Arduino Uno and ultrasonic sensors.  
The car detects obstacles in its path and automatically changes direction to avoid collisions.

This project demonstrates basic robotics, sensor integration, and automation control.

---

## 🧩 Tools & Components Used
- Arduino Uno
- Ultrasonic Sensor (HC-SR04)
- L298N Motor Driver Module
- DC Motors (2)
- Chassis & Wheels
- Battery Pack
- Jumper Wires

---

## ⚙️ Working Principle
1. Ultrasonic sensor measures distance to obstacles using sound waves.
2. If the obstacle is detected within 20 cm, the car stops.
3. The car moves backward for a short time, then turns right to avoid the obstacle.
4. The car continues moving forward until the next obstacle is detected.

---

## 🔧 Arduino Code (Upload to Arduino Uno)

```cpp
// Obstacle Avoiding Car using Arduino UNO
// Ultrasonic Sensor: HC-SR04
// Motor Driver: L298N

#define trigPin 9
#define echoPin 10

// Motor pins
#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5
#define ENA 6
#define ENB 7

long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  distance = measureDistance();
  Serial.print("Distance: ");
  Serial.println(distance);

  if (distance > 20) {
    moveForward();
  } else {
    stopCar();
    delay(300);
    moveBackward();
    delay(400);
    stopCar();
    delay(300);
    turnRight();
    delay(500);
  }
}

// Function to measure distance
int measureDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  return distance;
}

// Motor control functions
void moveForward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 150);
  analogWrite(ENB, 150);
}

void moveBackward() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  analogWrite(ENA, 150);
  analogWrite(ENB, 150);
}

void turnRight() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  analogWrite(ENA, 150);
  analogWrite(ENB, 150);
}

void stopCar() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
