# docker-builds

### Contact me - peter@alephinsights.com

A collection of docker builds I maintain for various projects.

**Not recommended for production environments.**  Presented as they are with no warranty or guarrantee. Use at your own risk. But if you get in touch with a problem, I'll see what I can do to help

Feel free to clone and hack about as you see fit.

## Inspiration and Acknolwedgements

My main inspiration for keeping these was a pal - he kindly suggested I stop trying to ballence various environment requirements on my native system!

Also [senbon](https://github.com/senbon/dockerfiles) who in turn was inspired by [Kaixhin](https://github.com/Kaixhin/dockerfiles) - some really good resources there.

## Connecting Collabratory to Docker instance.
Collabratory is great! Like GDocs for Jupyter notebooks! Collaborative coding. But sometimes the VM behind the scenes is a little weak. However it's possible to connect to a localhosted jupyter server. [How to here](https://research.google.com/colaboratory/local-runtimes.html). In sum, do this:
- `pip install jupyter_http_over_ws`
- `jupyter serverextension enable --py jupyter_http_over_ws`
- `jupyter notebook --NotebookApp.allow_origin='https://colab.research.google.com' --port=8888 --NotebookApp.token=''` Note the change here ...`token=''`

Not ideal because it's an open connection, but as long as your firewall stops external connections to 8888 it should be ok.

## Citation

If you find any of this useful please consider including a Citation

```
@misc{dockerfiles,
  author = {Peter Coghill},
  title = {alephinsights/docker-builds},
  url = {https://github.com/alephinsighs/docker-builds},
  year = {2017}
}
```
