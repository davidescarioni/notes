# GameMaker

## Funzioni

## approach()

Autore perso nella notte dei tempi (personalmente l'ho preso da [un video di Sara Spalding](https://youtu.be/2FroAhEsuE8)), serve per raggiungere un valore pian piano, una sorta di "lerp".

```GML
/// approach(_a, _b, _amount)
// Moves "a" towards "b" by "amount" and returns the result
// Nice bcause it will not overshoot "b", and works in both directions
// Examples:
//      speed = approach(speed, max_speed, acceleration);
//      hp = approach(hp, 0, damage_amount);
//      hp = approach(hp, max_hp, heal_amount);
//      x = approach(x, target_x, move_speed);
//      y = approach(y, target_y, move_speed);

function approach(_a, _b, _amount) {
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

## wave()

Autore perso nella notte dei tempi (personalmente l'ho preso da [un video di Sara Spalding](https://youtu.be/2FroAhEsuE8)), serve per cambiare un valore in maniera sinusoidale.

```GML
//wave(from, to, duration, offset)
// Returns a value that will wave back and forth between [from-to] over [duration] seconds
// Examples
//      image_angle = wave(-45,45,1,0)  -> rock back and forth 90 degrees in a second
//      x = wave(-10,10,0.25,0)         -> move left and right quickly
 
// Or here is a fun one! Make an object be all squishy!! ^u^
//      image_xscale = wave(0.5, 2.0, 1.0, 0.0)
//      image_yscale = wave(2.0, 0.5, 1.0, 0.0)
 
function wave(from, to, duration, offset) {
    var _a4 = (to - from) * 0.5;
    return from + a4 + sin((((current_time * 0.001) + duration * offset) / duration) * (pi*2)) * _a4;
}
```

## lerp_offset()

Funzione di lerp tra due valori nel quale è possibile aggiungere un offset per rendere più facile e veloce raggiungere il valore finale

//lerp_offset(_from,_to,_amount,_offset)
// Uses lerp between two values, but *_offset* is added to the difference, making it easier to reach final value

```GML
function lerp_offset(_from,_to,_amount,_offset = 1)
{
    var _diff =_to - _from;
    var _diff_sign = sign(_diff);
    if _diff_sign == 0
        return _to;
       
    _diff *= _diff_sign;
    return _from + _diff_sign * min((_diff + _offset) * _amount, _diff);
}
```
