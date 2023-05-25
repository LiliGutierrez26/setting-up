# setting-up

(defrule init
  =>
  (assert (monkey-at A))
  (assert (box-at B))
  (assert (banana-at C))
  (assert (on-floor A))
  (assert (on-floor B))
  (assert (on-floor C))
  (printout t "Escenario inicial:" crlf)
  (facts))

(defrule climb-box
  (declare (salience 10))
  (not (box-under-monkey))
  (box-at ?b)
  (monkey-at ?m)
  (on-floor ?m)
  (on-floor ?b)
  =>
  (retract (on-floor ?m))
  (retract (on-floor ?b))
  (assert (on ?m ?b))
  (assert (box-under-monkey))
  (printout t "El mono sube a la caja." crlf)
  (facts))

(defrule push-box
  (declare (salience 10))
  (box-under-monkey)
  (box-at ?b)
  (monkey-at ?m)
  (not (on ?b ?obj))
  =>
  (retract (box-at ?b))
  (assert (box-at ?obj))
  (retract (on ?m ?b))
  (assert (on ?m ?obj))
  (printout t "El mono empuja la caja hacia " ?obj crlf)
  (facts))

(defrule grasp-banana
  (declare (salience 10))
  (box-under-monkey)
  (banana-at ?obj)
  (monkey-at ?m)
  (on ?m ?obj)
  =>
  (retract (banana-at ?obj))
  (assert (monkey-has-banana))
  (printout t "El mono agarra el plátano." crlf)
  (facts))

(defrule done
  (declare (salience 10))
  (box-under-monkey)
  (banana-at ?obj)
  (monkey-has-banana)
  (on ?obj B)
  =>
  (printout t "Problema resuelto. El mono tiene el plátano." crlf)
  (facts))

(defrule fallback
  (declare (salience 1))
  =>
  (printout t "No se puede realizar ninguna acción." crlf))

(reset)
(run)
