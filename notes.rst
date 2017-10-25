- spent most of my time thinking about the questions to ask

  - predict just the accuracy from a beatmap?

  - predict the expected score for a hit object?

    - heavy skew in data; almost all 300

    - any random object is probably 300; need to capture difficulty

  - chose to predict magnitude of aim (x, y) and accuracy (time) errors

    - talked to Scott about this a lot

  - use volatility of error as a proxy for difficulty

    - more volatile is harder

    - originally tried truncated normal for the distribution

    - talked to Mir and learned about lognormal; plotting the data seemed to
      match this

  - originally tried to sample from the predicted distribution; too noisy /
    usually just 300

    - realized I can just use expected value with ``cdf`` or ``sf``.

- feature extraction

  - windows of hit objects and slider ticks

  - originally same amount of leading and trailing context (3)

    - 3 was too low; there is a lot of interesting trailing context

  - added longer trailing context, left leading at 3 because you only see a
    little bit in front of you at a time

    - maybe I should try 4 or 5 leading

    - 5 leading context was good; 7 did better

  - originally just used absolute x, absolute y, and relative time

- didn't inspect data close enough
  - curve errors

    - slight out of bounds Perfect sliders because the direction could get
      flipped

    - Linear -> Bezier bug caused massive numbers that destroyed the data when
      scaling (matryoshka)

    - looking through the beatmaps in the editor helped me get expectations for
      how to render a curve

    - using matplotlib to plot the curves helped verify correctness

    - Perfect curves don't flip their center point on hard rock

  - didn't scale the data which caused garbage results

    - tried scaling all the features together instead of column-wise; this was
      not good

  - double time replays getting misaligned adding a huge amount of noise to the
    dataset

  - replays from multiplayer with implicit 'no-fail'; there were replays where I
    just stopped playing part way through causing massive error

  - replays end with a weird (0, 0) click with some time near the end; this
    caused huge aim error spikes

  - adding the summary was a huge help; would have let me notice the outliers
    much sooner

  - plotting distributions showed interested patterns, do that first!

- cleaning valid data

  - capped aim error to 2 * circle radius and accuracy error to 1.5 * hit_50
    because things larger than that often come from skipping an object entirely
    which inflated the error

- keras stuff

  - using the functional API is much nicer

    - easier to reason about multiple outputs; can see the loss for each output

    - I am more used to graph based compute tools like blaze, pipeline, and dask

  - stacking lstm layers seems to allow me to capture more complicated sequences

    - still not totally sure on this; need to talk to Max

  - experimenting with mae vs mse for loss

    - I think mae will let the model make bolder shitmiss guesses but that is
      just a 'feeling'

    - mse seems to perform better for out of sample data

    - I think switching to use expected value instead of sampling makes the bold
      guesses a worse idea in general

  - trying more dropout for better out of sample performance

  - using sample weights to more aggressively account for error seemed to lower
    the loss a lot; this is probably because the dataset is heavily biased
    towards good scores

- tooling / workflow

  - all custom osu! specific stuff

    - good tools helped me work quickly

  - jupyter, keras, tensorflow, scipy, numpy

  - build keras yourself with -march=native; the sse instructions really sped
    things up

  - build model serialization early!

  - run models overnight or on your home server throughout the day; queue up
    tasks

    - take advantage of the serialized results to check the models later

  - add verbose options throughout your code for working interactively

    - make these options so your don't spam your logs when you deploy

  - tried aws p2.xlarge

    - trash performance; 37% slower than laptop on cpu

    - expensive

      - maybe reasonable option if you have an old computer?

      - just go to micro center and get a refurbished computer

  - look into gce

- operationalizing

  - exposed through web front end and irc bot

  - web frontend allows users to train

  - irc allows users to authenticate and ask for predictions / recommendations

- is this an improvement?

  - much more expensive to train than mlp

  - good manual features perform well

  - not much better (yet), is it worth it?

- talking to max:

  - add euclidean distance from previous circle

    - hyperbolic distance (hyperbolic metric)

    - pitch roll yaw through a circle (using 3 hit objects)

  - for mlp:

    - use skew and kurtosis

