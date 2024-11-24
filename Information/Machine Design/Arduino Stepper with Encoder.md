### Why combine a stepper motor with an encoder
Although steppers are designed to have good open loop position control abilities, there is always a chance of lost steps. By adding an encoder, the success of every step can be validated, and lost steps can be retried. For systems that require absolute positioning between power cycles (such as a rotary table where slots must be aligned with the substations), just adding an incremental encoder is not enough, as the system will have no information as to it's starting position. This necessitates the addition of an incremental encoder with z-index pulse to allows for homing or an absolute encoder to allows for absolute positioning at all times.

$\frac {200~steps}{1~rev} \frac{10~rev}{1~rotation} \frac{1~rotation}{8~slots} = \frac{250~steps}{1~slot}$

