Custom Components
=================

Vtex offers to create custom components in a simple way, using Reactjs. Here are some solutions to problems we have encountered during development

tsconfig.json
********************

It's neccesary make some initial configurations in your tsconfig.json

**tsconfig.json** contains TypeScript-specific options for our project.

If you have some problems trying to use external packages by npm or yarn just add the next line into **"compilerOptions"**.:: 

"noImplicitAny": false

Getting product data
********************

Naturally you will need to use the product information to work with it in your custom component. Here we show you how to implement it:

Go to your **ts** main page for your component and import the next one

.. code-block:: javascript

  import useProduct from "vtex.product-context/useProduct";

We provide you the next example: 

.. code-block:: javascript

  // grab product information
  const valuesFromContext = useProduct();
  const { selectedItem, skuSelector, product } = valuesFromContext;

   const images: any = useMemo(() => {
    let imagePaths;

    // if there's a image sku defined
    if (
      product &&
      product.items &&
      skuSelector &&
      skuSelector.selectedImageVariationSKU
    ) {
      const skuItem: any = product.items.find(
        (sku: any) => sku.itemId === skuSelector.selectedImageVariationSKU
      );

      if (skuItem) {
        imagePaths = skuItem.images;
      }
    }

    if (!imagePaths && selectedItem) {
      imagePaths = selectedItem.images;
    }

    return (imagePaths || []).map(generateImageConfig);
  }, [, product, skuSelector, selectedItem]);

Now **images** have product data and you will be able to use it

Let's explain the code above:

selectedItem contains:

.. code-block:: 

    {itemId: "60", name: "Tnf Black XS", nameComplete: "Parka The North Face Mujer Tnf Black XS", complementName: "", ean: "680975607741", …}
    attachments: []
    complementName: ""
    ean: "680975607741"
    estimatedDateArrival: null
    images: (4) [{…}, {…}, {…}, {…}]
    itemId: "60"
    kitItems: []
    measurementUnit: "un"
    name: "Tnf Black XS"
    nameComplete: "Parka The North Face Mujer Tnf Black XS"
    referenceId: [{…}]
    sellers: [{…}]
    unitMultiplier: 1
    variations: (3) [{…}, {…}, {…}]
    videos: []
    __typename: "SKU"


Detecting type device
*********************

Sometimes you need to know what device the user is using in order to return the appropriate component.

Below I show you how to do it: