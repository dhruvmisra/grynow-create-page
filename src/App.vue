<template>
  <div id="app">
    <div class="container">
      <h1 class="text-center">Create new Blog page</h1>
      <hr />

      <h2>File</h2>
      <div class="form-group col-6">
        <label for="fileName">File Name</label>
        <input
          type="text"
          class="form-control"
          id="fileName"
          placeholder="File Name (URL)"
          v-model="fileName"
        />
      </div>

      <h2>Head section</h2>
      <form>
        <div class="form-group col-6">
          <label for="title">Title tag</label>
          <input
            type="text"
            class="form-control"
            id="title"
            placeholder="Title"
            v-model="metaTitle"
          />
        </div>
        <div class="form-group col-8">
          <label for="metaDescription">Meta description</label>
          <textarea
            class="form-control"
            name="metaDescription"
            id="metaDescription"
            cols="30"
            rows="5"
            placeholder="Description"
            v-model="metaDescription"
          />
        </div>
      </form>

      <h2>Body section</h2>
      <form>
        <div class="row m-0">
          <div class="form-group col-8">
            <label for="blogTitle">Blog Title</label>
            <input
              type="text"
              class="form-control"
              id="blogTitle"
              placeholder="Blog Title (h1)"
              v-model="blogTitle"
            />
          </div>
          <div class="form-group col-4">
            <label for="author">Author</label>
            <input
              type="text"
              class="form-control"
              id="author"
              placeholder="Author"
              v-model="author"
            />
          </div>
        </div>

        <div class="form-group col-sm-12 col-md-8 mx-auto">
          <label for="blogContent">Blog Content</label>
          <div id="blogContent">
            <div class="form-group" v-for="item in blogContent" :key="item.id">
              <h5>{{ item.tag }}</h5>

              <div class="row m-0">
                <input
                  type="file"
                  accept="image/*"
                  class="col-10 mx-1"
                  placeholder="Content"
                  v-if="item.tag == 'img'"
                />

                <div v-else-if="item.tag == 'ul' || item.tag == 'ol'">
                  <ul v-if="item.tag == 'ul'">
                    <li v-for="(li, i) in item.li" :key="i">
                      <div v-for="input in li.inputs" :key="input.id">
                        <h6>{{ input.tag }}</h6>
                        <input
                          type="text"
                          class="form-control col-10 mx-1"
                          placeholder="Content"
                          v-model="input.text"
                        />
                        <button type="button" @click="deleteInput(input.id, li.inputs)" class="btn btn-outline-danger col-1 mx-1">&#10005;</button>
                      </div>
                    </li>
                    <div class="ml-2">
                      <button type="button" @click="addItem('p', item.li)" class="btn btn-outline-primary m-1">p</button>
                      <button type="button" @click="addItem('h3', item.li)" class="btn btn-outline-primary m-1">h3</button>
                    </div>
                  </ul>
                  <ol v-else>
                    <li v-for="li in item.li" :key="li.id">
                      <h6>{{ li.tag }}</h6>
                      <input
                        type="text"
                        class="form-control col-10 mx-1"
                        placeholder="Content"
                        v-model="li.text"
                      />
                      <button type="button" @click="deleteInput(input.id, li)" class="btn btn-outline-danger col-1 mx-1">&#10005;</button>
                    </li>
                  </ol>

                </div>

                <input
                  type="text"
                  class="form-control col-10 mx-1"
                  placeholder="Content"
                  v-else
                  v-model="item.text"
                />
                <button type="button" @click="deleteInput(item.id, blogContent)" class="btn btn-outline-danger col-1 mx-1">&#10005;</button>
              </div>
            </div>

            <button type="button" class="btn btn-outline-primary m-1" @click="addTextInput('p', blogContent)">p</button>
            <button type="button" class="btn btn-outline-primary m-1" @click="addTextInput('h2', blogContent)">h2</button>
            <button type="button" class="btn btn-outline-primary m-1" @click="addList('ul', blogContent)">ul</button>
            <button type="button" class="btn btn-outline-primary m-1" @click="addList('ol', blogContent)">ol</button>
            <button type="button" class="btn btn-outline-primary m-1" @click="addImage()">img</button>
          </div>
        </div>
      </form>

      <p>{{blogContent}}</p>

      <button class="d-block btn btn-danger btn-lg px-5 mx-auto my-5" @click="createPage">Create</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      fileName: "blog-blah",
      metaTitle: "",
      metaDescription: "",
      blogTitle: "",
      author: "",
      blogContent: [],
      blogInputs: 0,
      blogImages: 1,
      pageContent: ""
    };
  },
  computed: {
    fileNameWithExtention() {
      return this.fileName + ".html";
    },
    url() {
      return "http://www.grynow.in/" + this.fileNameWithExtention;
    }
  },
  methods: {
    addTextInput(tag, array) {
      console.log(array);
      array.push({
        id: this.blogInputs,
        text: "",
        tag: tag
      });
      this.blogInputs++;
    },
    addList(tag, array) {
      array.push({
        id: this.blogInputs,
        li: {
          inputs: []
        },
        tag: tag
      });
      this.blogInputs++;
    },
    addItem(tag, array) {
      if(tag == 'h3') {
        array.inputs.push({
          id: this.blogInputs++,
          text: "",
          tag: 'h3'
        },
        {
          id: this.blogInputs,
          text: "",
          tag: 'p'
        }); 
      } else {
        array.inputs.push({
          id: this.blogInputs,
          text: "",
          tag: tag
        });
      }
      this.blogInputs++;
    },
    addImage() {
      this.blogContent.push({
        id: this.blogInputs,
        src: '',
        tag: 'img',
        imgId: this.blogImages
      });
      this.blogImages++;
      this.blogInputs++;
    },  
    deleteInput(id, array) {
      const index = array.findIndex(item => {
        return item.id == id;
      });
      array.splice(index, 1);
    },
    createPage() {
      this.pageContent = `
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  
  <title>${this.metaTitle}</title>
	<link rel="icon" type="image/ico" href="../assets/logo.png" />
  <meta name="description" content="${this.metaDescription}">
  
  <!-- Bootstrap -->
	<link rel="stylesheet" type="text/css" href="../css/bootstrap.min.css">
	<!-- Custom Bootstrap -->
	<link rel="stylesheet" type="text/css" href="../css/custom-bootstrap.css">

	<!-- AOS -->
	<link rel="stylesheet" href="../css/aos.css" />
	<!-- Custom CSS -->
	<link rel="stylesheet" type="text/css" href="../css/styles.css">
	<link rel="stylesheet" type="text/css" href="../css/styles-blog.css"></head>
<body>

  <!-- Navbar -->
	<nav class="navbar navbar-expand-lg navbar-light bg-transparent">
		<div class="container">
			<a class="navbar-brand" href="../" style="padding-top: 0">
				<img src="../assets/logo_with_title.png" alt="GryNow logo" class="brand-icon">
				<!-- <h2 class="brand-name align-middle">Gry<span>Now</span></h2> -->
			</a>
			<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup"
				aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
				<span class="navbar-toggler-icon"></span>
			</button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <a class="nav-item nav-link" href="../social-media-influencer-platform-and-market-place.html">Creators <span class="sr-only">(current)</span></a>
          <a class="nav-item nav-link" href="../brand-marketing-management-agency.html">Brands & Agencies</a>
          <a class="nav-item nav-link" href="../case-studies.html">Case Studies</a>
          <a class="nav-item nav-link" href="../blogs.html">Blogs</a>
          <a class="nav-item nav-link" href="../influencer-management-agency-india.html">About Us</a>
        </div>
        <a class="btn btn-outline-primary rounded-pill ml-auto"
          href="../contact.php"
          role="button"
          >Contact Us</a>
      </div>
		</div>
  </nav>
  
  <div class="blog row">

    <div class="sticky-container col-sm-12 col-md-1">
      <div id="share-blog" class="text-center">
        <p class="">Share blog</p>
        <!-- Facebook -->
        <a href="http://www.facebook.com/sharer.php?u=${this.url}" target="_blank">
          <img src="https://simplesharebuttons.com/images/somacro/facebook.png" alt="Facebook" />
        </a>
        <!-- LinkedIn -->
        <a href="http://www.linkedin.com/shareArticle?mini=true&amp;url=${this.url}" target="_blank">
          <img src="https://simplesharebuttons.com/images/somacro/linkedin.png" alt="LinkedIn" />
        </a>
        <!-- Twitter -->
        <a href="https://twitter.com/share?url=${this.url}&amp;text=Grynow%20Influncer%20Marketing%20Blog&amp;hashtags=grynow" target="_blank">
          <img src="https://simplesharebuttons.com/images/somacro/twitter.png" alt="Twitter" />
        </a>
        <!-- Email -->
        <a href="mailto:?Subject=Simple Share Buttons&amp;Body=I%20saw%20this%20and%20thought%20of%20you!%20 ${this.url}">
          <img src="https://simplesharebuttons.com/images/somacro/email.png" alt="Email" />
        </a>     
      </div>
    </div>

    <div class="container">

      <h1 class="text-center">${this.blogTitle}</h1>

      <div class="blog-image">
        <img src="../assets/blogs/${this.fileName}1.jpg" alt="Influencer-marketing" class="w-100">
      </div>

      <div class="blog-content col-md-8 mx-auto">
`;
      this.pageContent += `<span class="author">- ${this.author}`;
      console.log(this.pageContent);
    },
    downloadFile() {
      let file = new Blob([this.pageContent], { type: "text/plain" });
      if (window.navigator.msSaveOrOpenBlob)
        // IE10+
        window.navigator.msSaveOrOpenBlob(file, this.fileNameWithExtention);
      else {
        // Others
        let a = document.createElement("a"),
          url = URL.createObjectURL(file);
        a.href = url;
        a.download = this.fileNameWithExtention;
        document.body.appendChild(a);
        a.click();
        setTimeout(function() {
          document.body.removeChild(a);
          window.URL.revokeObjectURL(url);
        }, 0);
      }
    }
  }
};
</script>

<style>
#app {
  background-color: rgb(248, 253, 255);
}
</style>
