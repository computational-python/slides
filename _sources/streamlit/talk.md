# streamlit

## howto

### doit

Streamlit

---

layout: false

## What is streamlit?

* a Python library
* displays graphics in the browser
* allows for some interactivity

---

## What is the hello world of streamlit?

~~~
import streamlit as st
st.write("Hello world!")

~~~

<img src="{{ base }}/img/helloworld.png">

---

## A dataframe

~~~
import numpy as np
import pandas as pd
import streamlit as st

sample = 10
values = np.random.randint(20000, 40000, sample)
genders = np.random.choice(['Female', 'Male'], sample)
units = np.random.choice(['Dept A', 'Dept B', 'Dept C'], sample)

df = pd.DataFrame(dict(values=values, genders=genders, units=units))

st.write("Sample data")
st.dataframe(df)
~~~

<img src="{{ base }}/img/df.png">

---

## A table

~~~
import streamlit as st
import numpy as np
import pandas as pd

dataframe = pd.DataFrame(
    np.random.randn(10, 20),
    columns=('col %d' % i for i in range(20)))
st.table(dataframe)
~~~

<img src="{{ base }}/img/table.png">

---


*   Streamlit apps are Python scripts that run from top to bottom
    Every time a user opens a browser tab pointing to your app, the script is
re-executed
*   As the script executes, Streamlit draws its output live in a browser
    Scripts use the Streamlit cache to avoid recomputing expensive functions,
so updates happen very fast
*   Every time a user interacts with a widget, your script is re-executed and
the output value of that widget is set to the new value during that run.
    Streamlit apps can contain multiple pages, which are defined in separate
.py files in a pages folder.

