# How to manage Jupyter notebooks on version control

This repository shows that how to manage jupyter notebooks with .rmd format.

Markdown is a lightweight file, based on markup language for formatting plain text so that we can use R Markdown format for Jupyter notebook.

### why we should use R Markdown

1. Jupyter notebooks' cons

When you git commit and push Jupyter notebooks, the disadvantages are the following:

- Jupyter notebooks generate files that may contain metadata, source code and media, etc. This makes these files poor, and much larger.

- Jupyter notebooks are stored in JSON format which makes it hard to do code review and to view diffs

2. Rmd format's pros

- lightweight file and less storage capacity

- easy to view diffs and code review

### How to create .Rmd file using Jupytext

Once you create a notebook, it will look like that as below:

![jupyter-notebook](ipynb_img.png =250x250)

The matter is that the file has metadata, source code, etc then the committed file will appear as JSON format
```
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[unstacking]\n",
      "bucket         0     1    2\n",
      "regiment                   \n",
      "Dragoons    55.0   7.0  NaN\n",
      "Nighthawks  37.0  24.0  NaN\n",
      "Scouts       2.0   3.0  5.0\n"
     ]
    }
   ],
   "source": [
    "print('[unstacking]')\n",
    "regimentbucketclicksum = groupbyregimentbucket['clicks'].aggregate(np.sum).unstack()\n",
    "print(regimentbucketclicksum)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAX4AAAFHCAYAAACmryeZAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAADh0RVh0U29mdHdhcmUAbWF0cGxvdGxpYiB2ZXJzaW9uMy4xLjEsIGh0dHA6Ly9tYXRwbG90bGliLm9yZy8QZhcZAAAgAElEQVR4nO3de5hVZf3A7wQEQ9GxBrgdmCPdLsjgIURcVNErImIp4A7gOMK2rozIp5M9/0VMLSNY7flgvR9eJPk/fxmMd9PSOoDfAr4fEQsj4jVEfH79TfRj4BDgQML/gB/
      ...
      lTkQsAU4HJkv6K0lI7xYRbwPnAvdLehR4FXi9lMdoY7e7gU/7y10rN0/nNCsgqVtErEqnfF4HzI+Iq7Ouy6yU3OM3W9/nJM0GngV6kMzyMdukuMdvZpYz7vGbmeWMg9/MLGcc/GZmOePgNzPLGQe/mVnOOPjNzHLmfwHVe+j5TU85VwAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "regimentbucketclicksum.plot(kind = 'bar', title = ' by Regiment, Bucket')\n",
    "plt.ylabel('clicks')\n",
    "plt.show()"
   ]
  },
...
```

We will create `.Rmd` using Jupytext.

#### Create .Rmd file using Jupytext

You need to perform this on all systems who will use the git repository with notebooks which means all your teammates must have jupytext configured.

1. Install Jupytext: `pip install jupytext --upgrade`

2. Jupyter config: If you don't have one yet, run `jupyter notebook --generate-config`

  - Mostly .jupyter is present in your home dir

3. edit `.jupyter/jupyter_notebook_config.py` and append the following:

```
c.NotebookApp.contents_manager_class="jupytext.TextFileContentsManager"
c.ContentsManager.default_jupytext_formats = ".ipynb,.Rmd"
```

4. Then restart Jupyter `jupyter notebook`

5. Open an existing notebook or create one then add the following `%autosave 0
` in the top cell and run the cell or save the notebook with `ctrl+S`/ `command+S`

6. You will see the `.Rmd` file has created.

#### 2 ways manage .ipynb and .Rmd

In my case, I saved only the `.Rmd` file on my remote server.

add `*.ipynb` into `.gitignore`.

- Note that if you want to remove stored `ipynb` on remote server, you can use `git rm --cached file`.

- Note that probably you can see `.ipynb_checkpoints` in the same path. you can refer to Stackoverflow, what's ipynb_checkpoints on this [page](https://stackoverflow.com/questions/36306017/should-ipynb-checkpoints-be-stored-in-git/39997938) and how to git ignore on this [page](https://stackoverflow.com/questions/35916658/how-to-git-ignore-ipython-notebook-checkpoints-anywhere-in-repository).

---

#### references:

- [Git Version Control with Jupyter Notebooks](https://towardsdatascience.com/version-control-with-jupyter-notebooks-f096f4d7035a)

- Stakoverflow: [Should ipynb checkpoints be stored in Git?](https://stackoverflow.com/questions/36306017/should-ipynb-checkpoints-be-stored-in-git/39997938)

- Stackoverflow: [How to git ignore ipython notebook checkpoints anywhere in repository](https://stackoverflow.com/questions/35916658/how-to-git-ignore-ipython-notebook-checkpoints-anywhere-in-repository)
