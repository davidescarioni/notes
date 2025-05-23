# GameMaker

## Funzioni

## Approach()

Autore perso nella notte dei tempi (personalmente l'ho preso da [un video di Sara Spalding](https://youtu.be/2FroAhEsuE8)), serve per raggiungere un valore pian piano, una sorta di "lerp".

```GML
/// Approach(_a, _b, _amount)
// Moves "a" towards "b" by "amount" and returns the result
// Nice bcause it will not overshoot "b", and works in both directions
// Examples:
//      speed = Approach(speed, max_speed, acceleration);
//      hp = Approach(hp, 0, damage_amount);
//      hp = Approach(hp, max_hp, heal_amount);
//      x = Approach(x, target_x, move_speed);
//      y = Approach(y, target_y, move_speed);

function Approach(_a, _b, _amount) {
    if (_a < _b) {
        _a += _amount;
        if (_a > _b)
            return _b;
    } else {
        _a -= _amount;
        if (_a < _b)
            return _b;
    }
    return _a;
}
```

## Wave()

Autore perso nella notte dei tempi (personalmente l'ho preso da [un video di Sara Spalding](https://youtu.be/2FroAhEsuE8)), serve per cambiare un valore in maniera sinusoidale.

```GML
//Wave(from, to, duration, offset)
 
// Returns a value that will wave back and forth between [from-to] over [duration] seconds
// Examples
//      image_angle = Wave(-45,45,1,0)  -> rock back and forth 90 degrees in a second
//      x = Wave(-10,10,0.25,0)         -> move left and right quickly
 
// Or here is a fun one! Make an object be all squishy!! ^u^
//      image_xscale = Wave(0.5, 2.0, 1.0, 0.0)
//      image_yscale = Wave(2.0, 0.5, 1.0, 0.0)
 
function Wave(from, to, duration, offset) {
    var _a4 = (to - from) * 0.5;
    return from + a4 + sin((((current_time * 0.001) + duration * offset) / duration) * (pi*2)) * _a4;
}
```