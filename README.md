## My Blog

1. Workflow

`cd _posts`
`touch yyyy-mm-dd-title-abc.md`
`jekyll build --incremental`
`jekyll serve`

2. Inside the file yyyy-mm-dd-title-abc.md start with -
  
`---`
`layout: post`
`title: "Hello World"`
`categories: [hello, world]`
`---`

3. Put all the images in `assets/img/*.png`. To include an image use
`[image name](/assets/img/image_name.png)`.
