# VerticalGroup. General usage.

## Create 

```
VerticalGroup vg = new VerticalGroup();
vg.setSize(width,height)
vg.setPosition(x,y)
vg.space(10); //Space between actors
```

## Add actors to the group

```
TextButton tb1 = new TextButton("text", textButtonStyle);
TextButton tb2 = new TextButton("text", textButtonStyle);

//Here I do the trick because I cannot set the height of buttons in vg other way.
Container container1 = new Container(tb1);
container1.size(w, h);
Container container2 = new Container(tb2);
container2.size(w, h);

vg.addActor(container1);
vg.addActor(container2);
```

## Add the VerticalGroup to a stage
`stage.addActor(vg);`
