# Modal

> A Vue2 Modal component.

```html

    'title' is the heading of the modal

    'open' is a boolean that controls whether or not the modal is displayed.

    The @close event is emitted from the component, but hiding the modal must be handled in the parent as well. This allows for programmatic opening and closing if the business logic requires it.

    Two slots: 'body' and 'footer'.

    <modal :title="'Whoa. A modal.'" :open="modalOpen" @close="modalOpen = false">
        <div slot="body">
            <p>This is body content. Neat.</p>
            <p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Laborum illo dolores ipsum ducimus esse quidem quod molestias recusandae! Natus maiores iusto numquam tenetur non laudantium, praesentium voluptatem itaque accusantium vel?</p>
        </div>
        <div slot="footer">
            <div>
                Footer content: <button @click="modalOpen = false">Close</button>
            </div>
        </div>
    </modal>

```

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```
